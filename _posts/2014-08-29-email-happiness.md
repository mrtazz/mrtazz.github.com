---
layout: post
title: My Rules for E-Mail Happiness
---

Over the last couple of days and maybe weeks a lot of my friends, coworkers
and most of all people I follow on twitter have been overly excited about the
beta availability of [an e-mail client][mailbox]. At first I was being
regrettably [very snarky about it][snarktweet] as in my opinion the mail
client in question has some serious privacy and availability issues. But as
more and more people got excited about it I took a step back and thought about
why I couldn't understand the excitement and why people would give up their
privacy for "better e-mail". And it dawned on me that I have never actually
seen e-mail as that problematic and annoying and that I actually like my
setup a lot. This is why I decided to share how I do e-mail and why it works
well for me (yes you may consider this one of those productivity blog posts).

### Setting the stage
Before I start I want to make very clear that I'm likely not a power email
user and what works for me might not work for you. This is also mostly about
how I manage my work email, as my personal email is low volume enough so that
it's probably not interesting. I also use a [somewhat esoteric e-mail
client][mutt] and while most of the things I'll talk about are generic, using
mutt makes it a lot easier for me. And to give you a ballpark number for
e-mail volume: I receive about 370 e-mails per day - your mileage may
vary.

### So how *do* I use e-mail?
The first two very important factors for me are filtering and [Inbox
Zero][inboxzero].  I am subscribed to a ton of mailing lists at work.
Everything I deem interesting (and I'm a super nosy person) I subscribe to,
but I have strict rules about what goes into my inbox:

- E-mails addressed directly to me
- My team's mailing list
- Mailing lists of teams I work closely with
- Low volume mailing lists (less than 5 messages a day)
- Important automated e-mails (e.g. from Nagios)

Everything else gets filtered into separate mailboxes. No exceptions. If a low
volume list gets more busy it gets filtered.

With this setup I check my inbox a couple of times a day. The frequency
depends on how busy I am obviously, but I check e-mail at most every 30
minutes and at least 5 times a day. And everything that is filtered I check
once a day up until once a week, depending on how important the mailing list
is. I also clear out all mailboxes at least once a week and archive all e-mail
in there. This usually takes about 5 - 20 minutes and I do it before doing my
weekly GTD review.

In mutt I also use the [solarized light theme][mutt_solarized] (as I do almost
everywhere else) which helps a lot as e-mails are color coded differently.
Read e-mails are gray, e-mails addressed to me directly or via cc are green
and messages from mailing lists are blue. That way I can open up mutt, take a
quick glance to see if I have new important email or if I can postpone going
through my email. This is sometimes so fast that my terminal emulator warns me
about the [shell being closed too fast again][muttwarn] and there might be
something wrong, which I still find hilarious. When I actually go through my
e-mail, I file them into mailboxes depending on whether I want to read them
later or already know that they need an answer and archive everything else.
Following the GTD principle if I can answer the e-mail in 2 minutes I do it
right away.  Otherwise I move it to the corresponding mailbox from which it
gets [pulled into my Omnifocus inbox][omnifocus_post]. All of this is done
with simple keyboard shortcuts that work on single or multiple messages.

I also have e-mail set up on my iPhone with the iOS built-in mail client via
IMAP. However I only ever skim e-mail on there and at most answer if it takes
me less than 2 minutes. I also have the labeling enabled that tells me whether
a messages was sent to me directly or via cc or just because I'm part of a
mailing list. That way I can also check very quickly if there is something in
there that potentially needs my attention.

### The important part
The most important part to take away from this is that **you don't need to
read all e-mail you get**. Especially in a big company there are enough things
going on that you can't possibly keep up with everything. And that's ok. In
addition to that what really works for me is using trusted clients that have
been around for a while. I have been using mutt for years and I have most of
its shortcuts in my muscle memory. When I came to work after a week of
vacation and had [6000 emails in my inbox][emailtweet] it took me 10 seconds
to clear out all the automated emails based on their from addresses and cut
the number of messages by 95%. The learning curve for mutt was pretty steep at
the beginning but it has payed off over the years and since it's open source I
know it will be around and not suddenly disappear (or at least it's unlikely).
I also love being able to write my e-mail in vim and am a big fan of plain
text e-mails. On my phone I also use the built-in e-mail client as it's likely
to stay and not completely disappear or get shut down either. My usage on the
phone is also light enough so I don't care about small changes in UX or
functionality with OS upgrades. I have yet to encounter a bad surprise after
upgrading my phone.

When it comes to dealing with stress and the feeling of being overwhelmed with
e-mail, the biggest change for me besides filtering was to turn off all
notifications. No e-mail that I receive will ever make a sound or make my
phone vibrate. There are no lock screen notifications on my phone besides
e-mail from people in my iPhone VIP list which is mostly family and even then
it just shows up. No sound, no vibration. I decide when I have time to read
e-mail.

As I said, most of these things are applicable no matter what e-mail client
you use. I happen to use mutt (set up similar to how it's explained
[here][homelymutt] and these are my [config files][muttfiles] in case you're
interested), but there are a ton of good and proven clients out there (I used
OSX Mail.app for years and always liked it). And plain old IMAP is honestly
pretty cool. But most of all - in my opinion - the biggest problems with
e-mail are social or rather psychological problems (trying to keep up with
everything, wanting to get notified all the time) and not technological ones,
and they can be solved.


[mailbox]: http://www.mailboxapp.com
[mutt]: http://www.mutt.org
[mutt_solarized]: https://github.com/mrtazz/muttfiles/blob/master/mutt-colors-solarized-light-16.muttrc
[snarktweet]: https://twitter.com/mrtazz/status/501909189577687040
[emailtweet]: https://twitter.com/mrtazz/statuses/486165858809815040
[inboxzero]: http://www.43folders.com/izero
[muttfiles]: https://github.com/mrtazz/muttfiles
[omnifocus_post]: http://www.unwiredcouch.com/2014/05/13/omnifocus.html
[muttwarn]: https://twitter.com/mrtazz/statuses/467405164790693888
[homelymutt]: http://stevelosh.com/blog/2012/10/the-homely-mutt/
