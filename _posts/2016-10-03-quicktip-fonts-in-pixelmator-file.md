---
layout: post
title:  "Quicktip: List of fonts used in a Pixelmator .pxm Document"
date:   2016-10-03
categories:
- quicktip
- pixelmator
---

Today I had to make changes to a Pixelmator document that I hadn't touched in
years. Unfortunately, I didn't seem to have all the used fonts installed and
I didn't find a way in Pixelmator to see a list of all fonts used / the missing
fonts. So I went into my terminal and tried to figure out the format of the
file which turned out to be unnecessary since there is a little command line
tool built into your mac called `strings` that allows allows you to pull all
the strings from a binary file.

Long story short, using `strings` and `grep`, it's very easy to extract the
fonts used in a .pxm Pixelmator document:

{% highlight bash %}
» strings test.pxm | grep fonttbl
{\fonttbl\f0\fswiss\fcharset0 ArialMT;}
{\fonttbl\f0\fswiss\fcharset0 ArialMT;}
{% endhighlight %}
