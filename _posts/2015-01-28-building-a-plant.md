---
layout: post
published: true
title: You're building a Plant
---

As an infrastructure engineer you're building a plant. Not literally of course
but it's also not too far off. There are actually a lot of parallels to
building a chemical plant. I used to work for a chemical company a couple of
years back and I have thought a lot about the similarities ever since I
started working at Etsy and with that on infrastructure for a running website.
This is why I decided to write down how I think about IT infrastructure
projects. However as it's been years since I set foot in a chemical plant,
most of the things I'm gonna mention and explain are probably outdated and
might be handled differently now. So take what I say with a grain of salt and
don't run off trying to build a chemical plant with this, please.

### Building a plant
The general setup of a plant is most often pretty structured on a high level.
You have a process that someone invented at some point. You burn sand or
combine acid to create a new product that has properties which are desirable.
Then you probably spent a lot of time improving and refining the process and
making the actual plant work better (this is where the vast amount of IP and
patents come from). And at some point you end up with a pretty good and
structured plan how to repeat it all over the world. Of course the process
refinements never stop but they get a little smaller over time.

Then at some point the time comes to build a new one, maybe at a new location
maybe at an old one. But you as the engineer (or project manager at that
point) get told that a new plant is needed. You likely have done that many
times before. So you're grabbing the documentation, maybe a project plan
template for it and you start planning. This is really where the first
important parts come together. A plant is build by tens or hundreds of people
all in all. But at this stage you have to make sure you're getting the right
people on the project for each component of the plant. Obviously there will
have to be some buildings. So you go to Facility Management and ask for
an engineer to work with you on the project. You don't have to know how to
construct buildings yourself, but it's an important part of the plant so you
get an engineer you can delegate the work to. Of course a dark, empty building
isn't that helpful. We are gonna put a ton of machinery in there. And all of
it needs power. So you're gonna get an electrical engineer on the project to
plan out the electrical infrastructure and hook the plant up to power. Someone
also has to actually install all that machinery, make sure you're choosing the
right parts and devices, work on the actual construction. This means, you
gotta get an engineer to supervise that. Once the plant is running there are
a lot of things that constantly need regulation. Water flow, temperature, air
flow, all those things need to be controlled and automated. You'll get an
automation, measurement and control engineer to take care of this of course.
And in the end this is also about a chemical process, so a chemical engineer
will also be on the project. With all these engineers on board, it's your job
to keep them all on the same page. You have to set up meetings, establish
communication and make sure everyone is included on updates since changes
might influence their area of work.

This is only an exemplary run down of what goes into building a plant.
Depending on what kind of engineer you are and the size and complexity of the
plant you might be doing one those things yourself. But for a big project you
might also just be the technical project lead and delegate all of those things
to others.


### Building infrastructure
So let's talk about the other kind of infrastructure projects. Where computers
are involved. Because they aren't that different. Sure they are often much
cheaper, you rarely get to work on a giant multi million dollar project. It is
also much easier and cheaper to experiment if you're writing software. You can
experiment with things before the whole project is done, change directions
much more quickly and generally see intermittent results with less hassle. And
you usually don't have to manage tens of people and contractor companies on
the project. On the other side you are usually trying something new, so you
don't have the security of having done this before multiple times. You have to
come up with a lot of things for the first time as you are trying to solve
problems with this new piece of infrastructure (this is why it might also be a
good idea to write down a [spec for your project][d2fn], to give you a better
picture of what parts are actually involved).

But the big thing both types of projects have in common is the collaboration
and delegation part. You most likely won't have to deal with facility
management, construction and maybe not even electrical power in your project.
But there are still a lot of parts. The software you plan to write or the
service you want to introduce is probably gonna run on some form of computer.
And although it has become way easier to spin up a virtual machine or
commission some hardware, you probably want to at least talk to an ops
engineer about it. There could be different classes of machines to choose from
that have proven to work better for different workloads. The requirements for
hardware (virtualized or real) could change if the scope or implementation
details of the project change. It could be that new machines are automatically
added to the monitoring system unless you tell it not to. And speaking of
monitoring: there should be some. And generally there is a lot of knowledge in
Ops about monitoring things. You also want to automate the setup for your new
service, write some Chef or Puppet to make it smooth to create new instances.
Which is probably another thing your ops team can help with. So ask for an ops
engineer to be on your project. Some infrastructure services also need to
persist data. Maybe you need a cool new database, although likely there is already an
existing place where you can put data. In any way this is something where an
Ops engineer (or DBA) can help with as well. As soon as you access and write
data, call to other services, maybe need some form of authentication or have a
naive implementation where you shell out in your PHP code you should have a
security engineer on board to make sure it doesn't end badly. Even if you
don't think you are, having someone from security on board for the project or
maybe even just review code changes a lot. Suddenly it's not that weird
service anymore that someone put into production. You get code review from a
very different angle and have the good feeling of everything being alright. Of
course your new thing also has a ton of tests. Unit tests are the simple thing
here, as you can probably hook them up to your existing CI infrastructure.
However there might be integration and end-to-end tests that you want to
create to increase confidence in changes when someone else (and also yourself)
works on the project. So when you start of the project, include someone from
your testing/QA/CI infrastructure team. You want to increase confidence in
changes with your new piece of infrastructure and engineers working close to
testing can definitely help. And speaking of confidence, there should be a way
of making things work in development as close as possible to how they are in
production. There should be a way to run it on a VM, on your laptop or some
other way where you can be sure changes you make in development work in
production. If you have a team that takes care of those things, get an
engineer from that team involved. Make sure they know that you care about
things working in development.

Recently, with the increased focus on cross team collaboration, there is a big
chance that you can do a lot of those things yourself. And you should, it's a
great way to learn (although that doesn't mean you shouldn't talk to the
engineers that work in those areas every day). But you can't do everything
yourself, so depending on the scale of the project and your familiarity with
all those areas, there will have to be delegation. And not all projects touch
all of those things. You might not have data to persist. Or you already have
an internal auth solution that you can plug in. However when in doubt it's
always better to err on the side of caution and include more people or ask
them if they think the project would benefit from it. The important part is to
take the time to talk to people and let them know you want their input on the
project or you want them to also work on the project. And then create a
mailing list, or a forum group or an IRC channel or however you handle
communication in your organization and invite all those engineers to join and
make it clear that project communication will happen there. You want them to
always know where they can get information about the project, its progress and
an up-to-date status in case they want to check if the project touches things
they work on. And not have to be afraid they might miss things.

Because you're building a plant. Kind of.

*Thanks to [Ben Hughes][bhughes] for reading drafts of this, giving me feedback and
being generally rad.*

[d2fn]: http://www.d2fn.com/2013/01/28/functional-specifications-for-infrastructure-engineers.html
[bhughes]: https://twitter.com/benjammingh
