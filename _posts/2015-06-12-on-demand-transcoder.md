---
layout: post
title:  "Building a realtime audio transcoder with Docker, FFmpeg and Flask"
date:   2015-06-12
categories:
- docker
- ffmpeg
- python
---


Recently I was faced with the issue of having a device capable of playing MP3s
from the network, but I only had MP4s available. Since the source files were too
many and changing daily, transcoding them in advance and effectively mirroring
them was not an option. The only solution would be to do it on demand. A great
opportunity to get fancy with Docker.

I quickly found a good ffmpeg Docker image, [cellofellow/ffmpeg](https://github.com/cellofellow/ffmpeg).
But how would I set it all up? I'm working on a Mac with boot2docker and docker-compose.
I thought about starting the ffmpeg containers over the remote api on my Mac's
Docker instance, but I wasn't really feeling comfortable with this setup. In my
opinion, the ffmpeg containers should somehow stay inside the application's
boundaries.


## Meet Docker-in-Docker

![](https://i1.wp.com/blog.docker.com/wp-content/uploads/2013/08/docker-meme.jpg?resize=499%2C323)

Docker introduced the privileged mode in version 0.6. It allows you to run
containers with many of the host machine's capabilities, regarding kernel
features and device access. [jpetazzo/dind](https://github.com/jpetazzo/dind)
makes it really easy to run a Docker host inside Docker. You can even run Docker
inside Docker inside Docker inside... Oh my.

The `docker-compose.yml` now looks something like this:

{% highlight yaml %}
docker:
    image: jpetazzo/dind
    privileged: true
    environment:
    - PORT=4444
{% endhighlight %}

We're going to use Flask in conjunction with gunicorn and a nginx reverse proxy
for the app itself.

Create a new folder `nginx` with this `Dockerfile`:


{% highlight dockerfile %}
FROM tutum/nginx
RUN rm /etc/nginx/sites-enabled/default
ADD sites-enabled/ /etc/nginx/sites-enabled
{% endhighlight %}

And the `nginx/sites-enabled/default`:

{% highlight nginx %}
server {
    listen 80;
    server_name transcodertest.productgang.com;
    charset utf-8;

    location / {
        proxy_pass http://transcoder:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
{% endhighlight %}


And finally add it to the `docker-compose.yml`:

{% highlight yaml %}
nginx:
  restart: always
  build: ./nginx/
  ports:
    - "8080:80"
  links:
    - transcoder
{% endhighlight %}


The web frontend itself will be a Flask application named transcoder, we can
add it to the `docker-compose.yml` already:

{% highlight yaml %}
transcoder:
  build: transcoder
  restart: always
  command: /usr/local/bin/gunicorn -w 2 -b :8000 app:app
  links:
    - docker
{% endhighlight %}


The Dockerfile for the transcoder app is quite simple. We're making use of the
`python:3.4.3-onbuild` image, which handles copying the `requirements.txt` and
`pip install`ing it. The final Dockerfile in `transcoder/Dockerfile` is simply:

{% highlight dockerfile %}
FROM python:3.4.3-onbuild
{% endhighlight %}

To communicate with the Docker host, we're using the `docker-py`. Apart from
flask and gunicorn that's our only requirement. Our `requirements.txt`:

    flask==0.10.1
    gunicorn==19.3.0
    docker-py==1.2.2


At first, we create a Docker client:

{% highlight python %}
from docker import Client
docker_client = Client(base_url='http://docker:4444')
{% endhighlight %}


In the handler for our transcode endpoint, we then create and start the ffmpeg
container:

{% highlight python %}
container = docker_client.create_container(
    image='cellofellow/ffmpeg',
    command=['ffmpeg', '-hide_banner', '-loglevel', 'quiet', '-i', url, '-codec:a', 'libmp3lame', '-b:a', '160k', '-f', 'mp3', '-'],
)
docker_client.start(container)
{% endhighlight %}


To get the output of ffmpeg, we're attaching to our container using the
`stream=True` option, in order to get back a generator rather than a string:

{% highlight python %}
output = docker_client.attach(
    container=container,
    stdout=True,
    stream=True,
    logs=True,
)
{% endhighlight %}


Since Flask supports streaming responses out of the box, getting the result to
the user is as easy as:

{% highlight python %}
return Response(output, mimetype='audio/mpeg')
{% endhighlight %}


That's all! You can now call our microservice with the url of an audio file
and get back a streamed transcoded MP3 version of it.

Our final app looks like this:

{% gist lukasklein/8b1252adfd4316a27524 %}
