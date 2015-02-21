---
layout: post
published: true
title: Learning to be On-Call
---

I stumbled upon this great [blog post about on-call][on-call-post] the other
day. It's a great article and you should definitely go read it first. It
prompted me to think about my own experience with being on-call and while I
don't have years of it, in the last 2.5 years I went from never having been
on-call before to being somewhat experienced in it. And especially the closing
quote of the aforementioned article about being scared of on-call really hit
home for me:

> Don’t be. While being on-call can be challenging, it is also very rewarding.

This is why I wanted to share my personal development and experience about
learning to be on-call. Because fundamentally I am convinced that it's not
only a necessary but also a rewarding thing to do. When I started at Etsy [a
bit over 3 years ago][first-etsy] I had never worked anywhere where I had to
be on-call. I was either too junior (engineers in training didn't have to be
on-call) or I was working on research and actual shipped software where there
were no production systems to look after. And when I applied for jobs I even
actively sought out job positions where I didn't have to be on-call a lot as I
saw it as a tedious, annoying and scary thing that I didn't want to do.

### How I got into this
Then when I joined Etsy every engineer had to sign up for a week of general
developer on-call per year (almost all of our on-call rotations are a week
long). This is the general on-call rotation for anything relating to the web
app of [etsy.com](https://etsy.com) where the knowledge of how things work an
ops engineer usually has doesn't suffice. We have amazingly good ops engineers
who can fix a ton of things themselves and if you have a really bad on-call
week you get paged once as a developer. Back then we were small (or big)
enough so that we could cover the year with every one being on-call at most
once. That wasn't too bad and so I put my week in about 6 months after I
started so I could learn some things about the web application before actually
being on-call. Since my team works on developer tooling and not the website
itself I chose this quite long period of time before my first on-call just
because I wouldn't have that much exposure to the app in my regular workday.
Lucky me, I happened to choose the week that included the weekend where we
wanted to upgrade our main SPOF database. So needless to say, I was really
scared of the on-call. I didn't have enough knowledge about the architecture
to know where things could break or debug it and I had never been on-call
before so even having to pay attention to whether or not I had cell reception
all the time was already making me nervous. I also hadn't been at Etsy long
enough to know how often I would get paged, how serious/time-sensitive a page
would usually be, whether or not I could be underground for 30 minutes to take
the subway home, which action should I use to tell PagerDuty that I'm working
on the incident, and what even would happen when I get paged and what to do
next. Most of this was just me being nervous. We had a lot of documenation
about being on-call and a lot of people to talk to. And I stayed up all night
during the database upgrade and didn't even get paged a single time. The whole
thing was planned really well and all the people who knew how the site worked
were already online and it turned out to be an invaluable learning experience
for me.

### Getting used to the game
Then after my first year or so we changed the on-call schedule a bit. We had
more engineers than we needed to fill the dev on-call rotation. We started to
have more specialized teams who would take on their own on-call and were thus
excluded from the generic one. So we switched to a system where you could
volunteer to be on-call every 4 weeks or so for a week. We set it up to be two
tiered so if you've never been on-call, you would be the L1 contact who gets
paged first. If you really can't get to a computer or have no clue what to do,
you can escalate to the L2 who is more senior and has experience with being
on-call. And then after 6 months you were released from the rotation and a new
round started where engineers could volunteer. I signed up immediately when
this got introduced. I wanted to learn more about what it means to be on-call.
The rotation was still super quiet and I almost never got paged. But just the
fact that I would now have to bring a charged phone, a Mifi and a laptop with
me wherever I went every 4 weeks made on-call less of a scary exception but a
scheduled routine. This took away a lot of my fears. I learned a lot about how
noisy of a rotation to expect, how to plan my day so that I would have cell
reception all the time, and how to react when I got paged. I still felt
awkward being on-call because I still almost never really knew the parts
involved when I got paged. I would work as a communication broker to call in
the right people but I could hardly ever fix things myself. Again I learned a
ton from watching the people I called debug and fix problems, especially in
parts of the app I had never touched before. But after a while it was also
somewhat unsatisfying to almost never get paged and then if a page happened,
not being able to actually do something about it.

### Level up: The Ops Rotation
In addition to that, by then I had worked on mostly infrastructure things. I
worked on a lot of Chef recipes, upgraded all of etsy.com to a new release of
PHP and reworked core parts of how software deployment worked. I touched all
the things that would wake up an ops engineer (but thankfully never did) and I
wanted to own up to the responsibility. But you only ever hear horror stories
about ops on-call from almost anyone. Because once you have an automated
system being able to wake you up at night (95% of Nagios alerts used to go to
the Etsy ops rotation back then) it changes on-call a lot. So to own up to me
having changed one of the biggest parts of our stack, I decided to sign up for
shadowing the ops on-call engineer for a week. I got my phone hooked into
Nagios. And [it got real][real]. I had added my email a couple of months
earlier already to get a feel for the alert volume and to know whether I broke
things I was working on but adding your phone is a different kind of real. I
chose a week where one of our senior ops engineers would be on-call and I got
woken up for everything for a week. It was pretty brutal. I got woken up in the
middle of the night and had no idea what Nagios wanted from me. I would log
onto IRC and get some information from [Marcus][marcus] what the alert was
about and how to fix it. I mostly felt helpless and super slow with debugging
what might be causing the production issues at hand. The week ended with a
couple of sleepless nights and a site outage on Friday. And I can't even begin
to explain how much I learned during that week. About systems I've never seen
before, about database replication, about hating disk space alerts, and how to
work after being deprived of sleep because of a busy on-call night. And the
learning was what got me hooked. A couple of months later I signed up for the
ops on-call rotation for good and have been part of it for over a year now.

### Long Story Short
I can honestly say leaning into on-call has been one of the best things I did
for growing as an engineer. It put me way out of my comfort zone and I had to
overcome a good chunk of impostor syndrome and fear for it. I write software
and design systems with a different view now. I have way more experience with
all the tools we use to manage our infrastructure - having to find something
in Chef at 3am really helps learning your way around it - and a better
intuition when it comes to adding things to it. When we added an on-call
rotation to my team I was super relaxed because I knew it wouldn't be anything
that I hadn't dealt with before in the ops rotation. It's a great feeling to
know that you are doing your part to keep things running and you can bond with
a lot of other people over sharing on-call pain and [how to make things
better][jnewland-monitorama]. It's not always fun. In fact it's fitting that
I'm writing this blog post slightly sleep deprived, while I'm on-call, have
been woken up almost every night since Friday, had to manage to find someone
to cover for me while I'm traveling for a whole day and had to hook up
notifications and mobile internet in another country for the first 2 days of
on-call. However I'm always happy when I'm done with another on-call week and
can look back on the things I learned. After all it's the challenges through
which we grow.


[on-call-post]: https://medium.com/@thematthewgreen/on-call-dont-be-scared-4eef4ff2928f
[first-etsy]: https://twitter.com/mrtazz/statuses/147115673137577984
[jnewland-monitorama]: https://speakerdeck.com/jnewland/optimizing-ops-for-happiness
[marcus]: https://twitter.com/ickymettle
[real]: https://www.youtube.com/watch?v=uvqJ1mTkEuY
