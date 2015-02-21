---
layout: post
title: "gtd-couch - GTD as a couchapp"
published: true
---

### Why another GTD app?
Over the last years I have tried a lot of task management applications. I was
always trying to improve the status quo, where I forgot some things I wanted to
do and didn't have a possibility to jot things down when I should have. For the
last 2 years [Things][2] was my task manager of choice. With the iPhone
companion app I could add tasks on the go and sync them to the Things database
on my MacBook. The fact that it was only possible in the local network was
acceptable, since cloud sync was [announced with priority][3].

Over time I added an [iMac][4] and an [iPad][5] to my setup. This made
t(T)hings a bit more complicated. Syncing between the iMac and MacBook is much
more complicated. I chose to sync my Things database file via my [DropBox][6]
account to all my computers. But this means that only one instance of Things
can be running at a time. If I forget to close the app on one machine, the data
will probably not be up to date or even overwritten.  The iPad can sync natively with
the Mac application. But then again only via the local network. I also didn't
want to pay half of the price of the desktop application for a solution I'm not
satisfied with. And I didn't want to setup some VPN/Wide Area Bonjour solution
just to sync my tasks. And there is still no over-the-air sync solution in
sight.

The main problem is apparently that this syncing and scaling stuff is [really
hard][7]. Which is also the reason why systems like [CouchDB][8] don't exist. Don't
get me wrong, I don't think you can solve this (in a satisfying way for
developers and customers) by just throwing a piece of technology at it. But I
also don't think this is we-have-to-think-about-it-for-2-years hard. Needless
to say I am a bit disappointed how this turned out. And due to this I also used
Things less and less, which didn't really improve the situation.

### The alternatives

So I started to try out other task management solutions. The obvious choice was
[OmniFocus][9]. It has great syncing support and device integration. However I
just want a simple solution and OmniFocus can do pretty much everything ever
imagined regarding task management. So it is a bit bloated to use. Other
notable choices are [Todo for iPad][10]/[iPhone][11] in combination with the
[online syncing service][12]. There is no Desktop app however and I have the
feeling, that the data is rather closed in.

One really good looking alternative is [wunderlist][13] by the Berlin based
company [6 Wunderkinder][14]. There is a Desktop and iPhone app and there is
also online syncing built in. This is really a promising todo manager with a
clean interface. But I also wanted the possibility to have my data in my own
personal cloud.

### CouchDB to the rescue
As I had started to work a lot more with CouchDB lately, I decided to built a
GTD manager as a couchapp. This meant I get replication, simple storage and the
REST API (almost) for free. Additionally it supports a completely decoupled
approach to interface with the data. The first step is of course to write a
couchapp to be served directly from CouchDB. This can then be used to access
the data from everywhere. And can also be replicated together with the data to
any device supporting CouchDB.

For the next steps the CouchDB HTTP/JSON API also enables the easy creation of
clients for the data in almost any language or technology. Therefore a console,
native OSX or iPhone client or even a much better web client can be created
rather easily. And all this while your data remains under your control. Just
[get][15] a CouchDB and get started.

### Use this now?
That being said, [gtd-couch][16] - the couchapp mentioned before - has just
reached version v0.1.0. It already features basic functionality and is pretty
usable.  Try it out if you're interested, I use it as my task manager already.
There are also some outstanding issues and new features which will be
implemented as time allows.

This does not mean that I won't try out the Things cloud sync solution, when
(if) it finally comes out. I still think it has really great design and a
simple interface. But for now I'll enjoy being able to interface with my data
as I want to and host it where I want to.



[1]: {{ page.url }}
[2]: http://culturedcode.com/
[3]: http://culturedcode.com/things/blog/2009/08/this-is-not-a-roadmap.html
[4]: http://unwiredcouch.com/2009/12/24/first-weekend-with-endless-screen-real-estate.html
[5]: http://twitter.com/mrtazz/status/24500270704
[6]: http://db.tt/IizMFIF
[7]: http://culturedcode.com/things/blog/2010/12/state-of-sync-part-1.html
[8]: http://couchdb.apache.org/
[9]: http://www.omnigroup.com/products/omnifocus/
[10]: http://www.appigo.com/todo/ipad
[11]: http://www.appigo.com/todo
[12]: https://appigotodo.appspot.com/
[13]: http://www.6wunderkinder.com/wunderlist/
[14]: http://www.6wunderkinder.com/
[15]: http://www.couchone.com/get
[16]: https://github.com/mrtazz/gtd-couch
