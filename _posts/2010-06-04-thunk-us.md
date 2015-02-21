---
layout: post
title: Thunk.us Python wrapper
published: true
---

Lately I have become more and more interested in APIs (especially on the web).
As a first manifestation of this interest I converted the simple python script
to add articles to [instapaper][instapaper] I wrote some time ago into a usable
[python library][instapaperlib] utilizing the full API.

Then, while reading some blog posts about continuous integration and build notification,
I stumbled upon [thunk.us][thunkus]. It is a simple status management service. You
register your thunk there, poke it to set the status and everyone who needs to
(i.e. knows the ID) can check it. It can easily be used to notify about succeeded/failed
builds, status of queues or coffee supply or anything else for that matter. Allthough I
had no immediate usage for such a system, I read the API definitions and found them to
be interesting enough to build something upon it.

Enter [thunkapi.py][thunkus_github]. A python library with command line client to interact
with the thunk.us service. I built the library with ease of use in mind (and I think I
somewhat succeeded). One can easily provide states, payload and a UID (or list of UIDs where
applicable) to the exposed methods and get a clean python dict object in return.

I will still have to see if I have a productive use for the thunk.us service. But the fun of
building the library was motivation enough. So have fun with it, use it if you like it
and if you dislike something about it: [Fork me!][thunkus_github]



[thunkus_post]: {{ page.url }}
[instapaper]: http://instapaper.com
[instapaperlib]: http://pypi.python.org/pypi/instapaperlib
[thunkus]: http://thunk.us
[thunkus_github]: http://github.com/mrtazz/thunkapi.py
