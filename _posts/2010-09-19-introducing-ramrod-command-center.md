---
layout: post
title: "Introducing Ramrod Command Center"
published: true
---


I have [finally][2] registered my Master's thesis. This means in 6 month
my university life is over and the hard and cruel reality begins. I am writing
my thesis at the [Chair of Computer Networks][3] about localization of nodes
without external positioning information. I am going to port the existing
algorithms to Unix platforms and create a server environment to be able to use
the localization also in networks without direct communication.

The resulting (localization) software has to run on different platforms such as
Windows, Unix and the iPhone. This is why I wanted a build system with
continuous integration, where the software is built and tested on these
platforms. For continuous integration I normally use [integrity][4] or
[cijoe][5], which are simple but therefore don't support notifying builds for
several platforms. [Hudson][6] has support for agents, but as far as I found
out, they have to be hudson agents. For my Master's project I wanted to be able
to use integrity as well as cijoe as agents and have a simple central command
instance, which notifies them.

### Ramrod
This is why I built [ramrod][7]. It is a small sinatra application, which acts
as a sort of CI control center. Ramrod can be notified to build a project via
simple HTTP POST. All registered agents are then notified subsequently. The
agents have to be configured to notify the result back to ramrod, where all the
results are then displayed. This can be easily done with cijoe and integrity.
Ramrod itself has a simple structure and can be deployed to any ruby
hosting platform.

The project is still in a very early stage and there are a lot of things which
have to be improved (or even implemented), such as a notification system,
authentication and of course a much better design. But I think the initial
release v0.1.0 is already quite helpful and usable.


[1]: {{ page.url }}
[2]: http://twitter.com/mrtazz/status/24040433982
[3]: http://cone.informatik.uni-freiburg.de
[4]: http://integrityapp.com
[5]: http://github.com/defunkt/cijoe
[6]: http://hudson-ci.org
[7]: http://github.com/mrtazz/ramrod
