---
layout: post
title: Google Reader to Instapaper bridge
---

### The dilemma

For some years now I am using [Google Reader](http://google.com/reader) as my
reader of choice for news feeds. I still think it's the best way to get daily
news (for me), although I do recognize that I don't read every feed as
thoroughly as I used to anymore. I always use the starred items section as a
sort of "save for later" storage, so I can open all the unread stories in my
browser tabs and read them when I have time. This worked out ok, but in the
last months I got more and more used to the
[instapaper](http://www.instapaper.com) service, which provides a simple web
frontend to save websites for later and a great [iPhone
app](http://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=284942713&mt=8).
This led to the situation, that I still starred items in Google Reader, but
then had to open the pages and manually save them to instapaper.com and unstar
them in Google Reader. An alternative is the builtin Google Reader option to
share to instapaper, which also needs some manual actions and you still have
to unstar the item afterwards.

### The solution

So I decided to build a script, which would run nightly on a server and pull
all starred items out of Google reader and into instapaper. Since I am trying
to improve my python knowledge whenever I can, I decided to build a python
implementation. Since I also wanted to dig into the (unofficial) Google Reader
API, I decided not to use the very good existing [python
framework](http://code.google.com/p/pyrfeed), but to utilize the API myself. I
had already written a small [instapaper
library](http://github.com/mrtazz/InstapaperLibrary) before, which I mainly
used to save articles for later on the command line.

So I sat down and started to write [the
bridge](http://github.com/mrtazz/instareader.py). All in all it took
me a bit more than a month (due to university life, work and exams) to finally
get a basic version working, which is able to retrieve starred items, save
them to instapaper and then remove the star from instapaper. The script is now
run every hour on a server, which is enough for my needs of instapaper being
up-to-date. Also I have more choices on iPhone RSS readers with Google Reader
syncing now, since Instapaper support is not a requirement for the app
anymore. There are still (as always) some things to do, but at least the
manual open,save,unstar actions are no more now.
