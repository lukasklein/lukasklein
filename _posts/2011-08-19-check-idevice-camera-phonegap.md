---
layout: post
title:  "Check whether your iDevice has a camera built in in Phonegap"
date:   2011-08-19
categories:
- phonegap
---


Earlier this week I discovered [Phonegap](http://www.phonegap.com/) when I got a beta-invite to their new
[build-service](http://build.phonegap.com/). To those who never heard of Phonegap before, it is:

	an HTML5 app platform that allows you to author native applications with web technologies and get access to APIs and app stores

One of the awesome things about Phonegap is the extensibility with plugins
caused by the nature of open source. Even though the built-in features of
Phonegap already are very well-engineered, there are some cases in which you
have to write your own plugin in order to achieve the functionality you want.

For example, the camera-API doesn't offer the ability to check, whether the
device contains a camera. In a project I'm currently working on the user can
take a picture, but what if he's using an older iPod touch or an iPad 1? In
this case I'd like to hide the "Take a picture" button and instead only display
a "Choose from library" button, as you probably know it from other iOS apps.


It's very easy to test this in Objective-C:

{% highlight objective-c %}
bool hascamera = [UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera];
{% endhighlight %}

But it's some extra effort to make this available in Phonegap. I'll be posting
the .h and .m files, which you have to put inside your Plugins-directory and
the .js file which you'll be importing in your HTML-page (therefore you have
to put it in the www-directory). For more information on how to install plugins
in Phonegap for iOS, see [http://wiki.phonegap.com/](http://wiki.phonegap.com/w/page/43708792/How%20to%20Install%20a%20PhoneGap%20Plugin%20for%20iOS)

Here's a quick demo of how to use it:

{% highlight javascript %}
window.plugins.CameraAvailable.hasCamera(function(result) {
	if(result.available) {
		var buttons = ["Take Photo", "Choose From Library", "Cancel"];
	} else {
		var buttons = ["Choose From Library", "Cancel"];
	}
});
{% endhighlight %}


And here's the code:

{% gist lukasklein/f99d5e80a2880765a283 %}
