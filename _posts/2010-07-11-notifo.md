---
layout: post
title: Notifo
published: true
---

[Notifo.com][notifo] is a web service which enables you to send push
notifications to your mobile device (at the moment there is only support
for the iPhone). This [blog post][notifo_announce] first called my attention
to the service. I have used [prowl][prowl_url] (a similar service based
on [growl][growl_url]) before and I was instantly interested in this
somewhat more versatile notifo.

On the same evening I decided to build a [python library][github_notifo-py],
to be able to easily notify users from python applications. I also thought it
would be cool to get push notifications from your CI server about your builds.
So I implemented an [integrity][integrity_url] notifier which is now available
in the main line.

At the moment I personally use notifo to get notified about twitter mentions
via [push.ly][pushly_url], website changes via [femtoo][femtoo_url] and commits to some of my
github projects via a [service hook][notifo_hook]. Once I have found suitable
hosting for integrity for my projects, I will also use the integrity notifier.
And I am very excited to see what else will be build upon this service.


[notifo_post]: {{ page.url }}
[notifo]: http://notifo.com
[notifo_announce]: http://paulstamatiou.com/notifo-yc-w2010-gets-a-co-founder-me
[prowl_url]: http://prowl.weks.net/
[growl_url]: http://growl.info/
[github_notifo-py]: http://github.com/mrtazz/notifo.py
[integrity_url]: http://integrityapp.com/
[pushly_url]: http://push.ly
[femtoo_url]: http://femtoo.com
[notifo_hook]: http://github.com/github/github-services
