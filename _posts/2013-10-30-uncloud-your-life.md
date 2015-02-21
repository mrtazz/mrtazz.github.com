---
layout: post
title: "Uncloud your Life"
published: true
---

There has been a lot of talk lately about privacy in the cloud and owning
your own data. I'm not linking any articles here, since there are so many
and I don't think anyone has missed it. However it spurred a new and awesome
debate about hosting your own applications and thinking about where your data
is stored and who manages it. I have thought about writing this for a while
and always felt there wasn't enough to write about. But in the spirit of
sharing and getting back into writing I decided to do it nonetheless.

The setup I'm describing has grown pretty organically and is heavily based on
what I use and how I work everyday. This is also probably a bit too technical
to be considered a general purpose manual. But that will have to do for now. I
am also a heavy FreeBSD user, as it makes a lot of things easier and more
enjoyable for me. So my setup is also very biased towards that.

### Email
Maybe one of the most important parts is email. I switched from hosted email
providers to self hosted (first on a friend's server) in 2005, when the 12MB
Inbox I had wasn't big enough anymore and before GMail was widely available in
Germany (at least I only knew one person with a GMail account back then). So
nothing has changed for me there. I also think email is considered to be one
of the more painful things to self host although I don't think this is true. I
run a setup based on [FreeBSD][freebsd], [sendmail][sendmail] and the [Dovecot
IMAP server][dovecot] which is not very complicated to set up. Especially the
FreeBSD/sendmail part literally takes 10 minutes. I don't really run spam
filtering since it hasn't been a problem (I do filter some known spam
addresses in my procmail rules though). I read my email in [mutt][mutt] on the
laptop where it is synced with [offlineimap][offlineimap] and also run mutt in
a tmux session on my mailserver to access it from anywhere. On iOS devices I
use the built-in Mail application and have come to love [Triage][triage] for
quickly going through email when I have a minute.

### Calendars and Contacts
Another very important aspect of my daily synced data are calendars and
contacts. Especially with the iPhone and iPad being in constant use, I want
that data to be synced everywhere. I used to use iCloud for that and it works
beautifully and I wanted something which works equally flawless. After some
trial and error I found [ownCloud][owncloud] which provides CalDav and CardDav
services as well as general WebDAV. The setup guides are really good and
include most of the common clients. I nevertheless ran into some problems
with the initial setup on iOS and OSX clients because of when and where they
expect slashes or protocol headers. However this is a
configuration/documentation issue, which is annoying but can be solved.

### File sync
I used to use [Dropbox][dropbox] a lot. I loved the simplicity and being able
to have files in sync everywhere. I even put my git repos in there at some
point so I could continue working from every computer I used. With time I used
it less and less as I simplified my workflows a lot but it was still important
to have a proper file sync solution, mostly for convenince options like
syncing [Alfred][alfred] preferences. But I wanted to get all my documents out
of a location that somebody else had under control. Thankfully ownCloud also
comes with a client to sync the WebDAV directory between computers. So I
basically set that up and copied everything over from Dropbox. It has been
working really well so far, though I don't have heavy requirements for syncing
and files in there don't change that often.

### GTD/Todo tracking
I track everything in [OmniFocus][omnifocus]. Literally. Work stuff, personal
stuff, movies I want to watch, books I want to read, it pulls in GitHub issues
and Jira tickets that are assigned to me, I plan blog posts I want to write
and talks I want to give in there. I extensively use custom perspectives to
get data out. It's safe to say that it's an important piece of software for
me. Luckily Omni products support syncing via WebDAV and have been for a
while. Thus it was very easy to switch from the hosted Omni Sync Server, which
works flawlessly, to just use WebDAV endpoint of ownCloud. I have since also
looked around to find out if there are alternatives to OmniFocus if I ever
wanted to switch away from OSX. Sadly it seems to be that self hosting is
rarely an option for any app and I have only found a handful that even
provided a synchronisation mechanism that does not involve DropBox, Apple or
their own cloud sync solution.

### Note taking
Note taking was also an important part that had to continue to work for me. I
don't take a lot of notes all the time. But when I need to jot something down,
it must not matter whether I'm on my phone or in VIM on my laptop. I was a
very happy [Simplenote][simplenote] customer and still think it's the best
cloud based note taking platform there is. I even wrote a [VIM
plugin][simplenote.vim] for it so I'd never have to leave my trusty editor.
This also meant a solution that would replace it needed a decent iOS client,
notes I can access from VIM, support for Markdown and a syncing engine that is
ideally based on WebDAV, since I was already running that. And after some
searching I actually found this unicorn of note taking solutions. It's simply
called [Notebooks][notebooks] and it's a simple app that displays the folders
and files in a WebDav directory, let's you edit text files and view them in
Markdown mode. And even take and attach pictures. It comes as a Universal App
for iPhone and iPad and has an OSX client in a beta version, which I don't use
because I can just edit all the files in VIM. Which makes me very happy.

### Password syncing
The only application I haven't found a satisfying self hosted solution yet is
password syncing. I use [1Password][1password] and am very happy with it.
However the only non-LAN solutions for syncing that it provides are Dropbox
and iCloud. So I switched to Wi-fi sync for my passwords. It's not ideal and
there will come a point where I am on my iPad and don't have a password there
and am too lazy to open the laptop to sync. However since all passwords for
my crucial services are already synced this won't be the end of the world and
can very likely wait until I am on a nother device or have both the laptop and
the iPad open. So I'm not 100% happy with it but it is one of those "good
enough" solutions.

### IRC and Instant Messaging
Being able to idle on IRC and have a proper chat client at hand everywhere has
always been important to me and for that I have run terminal based clients in
a screen or tmux session for years now. Since I (similarly to email) never
used any of the cloud based solutions, I was already running [ZNC][znc] and
[Bitlbee][bitlbee]. And since the changes in GTalk earlier this year which
broke a lot of stuff for me, I also already had a [Jabber account][jabber]
which I was using for chat and OTR.

### Backup
How to handle backups was one of the bigger concerns I had. Now that I would
be hosting all my data I needed a proper plan so when one of my servers dies
I'm not losing everything that was on there. Like probably almost every Mac
user, I used to use [Arq][arq] to backup my laptop to an encrypted S3 bucket.
However that was only ever the client side. And I was happy with it because
it included my mail folder and thus I had a backup of my email. And when I
stopped using that to not push all my date to S3 I also didn't backup my email
anymore. After some thought it was clear to me that I wanted to have a backup
in a location with as much control as possible. I decided to buy an [HP
Microserver][microserver] and put it in my apartment. It runs FreeBSD
(surprise!) and has a 2x2TB encrypted ZFS RAID. The backup location for each
of my machines on that RAID is an independent filesystem so I can snapshot it
regularly and go back in time if I have to. The server pulls in data from my
servers via rsync and that's how I do backups. It's less automated than I want
it to be right now and I still have to configure it to server as a TimeMachine
destination for my laptop. But this is already a pretty good solution for me.

### Where I still use the cloud
I've extensively talked about how I moved my data into self hosted
applications and what I use for those use cases. However that doesn't mean
that I'm completely free of cloud based applications. Obviously there are a
variety of applications that don't support this yet or where it's not even
something that would work without changing the product a lot. That means I
still use Dropbox to sync [Papers][papers] or automatically pull in pictures
from Instagram. Since Google Reader died I switched to [Feedbin][feedbin] and
have no intention to stop using it, I have my Kindle books at Amazon, my music
in the iTunes Cloud, I use a variety of infrastructure software as a service
to [monitor my servers][monitoring] and I run my [public image
sharing][imagesharing] and [custom URL shortener][katana] on S3 and Heroku and
this blog on GitHub Pages. The difference for me is, that most of this data I
don't necessarily regard as private as the ones I pulled into my own hosting.
I will probably experiment with how I can do some of this on my own in the
future, but it is less important to me right now.

### Why are you telling me all this?
As I said in the first section, this is not considered a manual of how to host
your own data. While I try to keep my [Chef cookbooks][cookbooks] for this
stuff up to date, they are very custom tailored and probably not of great use
for everybody. If you want to get started and host your own data, I highly
recommend checking out [Alex Payne's Sovereign Project][sovereign]. It's an
Ansible project which installs a lot of the things I've been talking about
here and is definitely much easier to get started with. I do hope though I was
able to share some ideas and make hosting your own data sound a little less
scary.

I also realize that even with an easy to get started guide and a collection of
Chef recipes this is not something every person can run and you need some
understanding of (and tolerance for) running your own services. There has been
some work going on for some time to make it easier to host your own services
and even have decentralized applications. The newest one I am aware of is
called [Grand Decentral Station][decentralize] and looks very promising. I
would love to see some of these ideas flourish and be pushed forward. And
maybe have a future in which we can not only pay people to run services for
us, but also to develop services we can run ourselves as easy as it is to set
up a TV or a Roomba today.


[sovereign]: https://github.com/al3x/sovereign
[omnifocus]: http://www.omnigroup.com/omnifocus
[freebsd]: http://www.freebsd.org
[sendmail]: http://www.sendmail.com/sm/open_source/
[mutt]: http://www.mutt.org
[dovecot]: http://www.dovecot.org
[triage]: http://www.triage.cc
[owncloud]: http://owncloud.org
[offlineimap]: http://offlineimap.org
[dropbox]: http://dropbox.com
[alfred]: http://www.alfredapp.com
[simplenote]: http://simplenote.com
[simplenote.vim]: https://github.com/mrtazz/simplenote.vim
[notebooks]: http://www.notebooksapp.com
[papers]: http://www.papersapp.com
[1password]: https://agilebits.com/onepassword
[arq]: http://www.haystacksoftware.com/arq/
[microserver]: http://www8.hp.com/us/en/products/proliant-servers/product-detail.html?oid=5336619#!tab=features
[feedbin]: https://feedbin.me
[monitoring]: http://www.unwiredcouch.com/2012/09/15/getting-started-with-monitoring.html
[cookbooks]: https://github.com/mrtazz/cookbooks
[jabber]: http://web.jabber.ccc.de
[znc]: http://wiki.znc.in/ZNC
[bitlbee]: http://www.bitlbee.org/main.php/news.r.html
[decentralize]: http://decentralize.it
[imagesharing]: https://github.com/roidrage/s3itch
[katana]: https://github.com/mrtazz/katana
