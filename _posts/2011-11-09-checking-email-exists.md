---
layout: post
title:  "Checking if an email address exists"
date:   2011-11-09
categories:
- email
- smtp
- python
---

Today I was faced with an interesting question: How can you tell if an
email-address exists? Yep, not if it's valid (that's easy with some
regexp-magic), but if it actually exists. I came up with a simple
Python-solution.

What if we want to check, if, for example, *somefoobarstuff@gmail.com*, exists?
It's a valid email address, but it obviously doesn't belong to any account. My
solution uses SMTP, the protocol that is used to send emails.

At first we have to find the MX-record of the domain - it tells you what mail
server you have to use. I'm using the [Python DNS Library](http://sourceforge.net/projects/pydns/) to get the actual
records and [smtplib](http://docs.python.org/library/smtplib.html) to try to establish a connection.

I'm maybe a bit lazy, but instead of reading all the smtplib documentation I
decided to implement the [SMTP protocol](http://en.wikipedia.org/wiki/SMTP#SMTP_transport_example) on my own. We basically only need to
go as far as to the RCPT TO command, to which the server replies with an 550
error if the recipient does not exist.

I'm maybe bad at describing things, but I hope you can find my code helpful for
whatever you're planning. But please be nice and don't use it for spamming :)

{% gist lukasklein/0ea94303d013fd6dae18 %}
