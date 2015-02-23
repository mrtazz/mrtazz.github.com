---
layout: post
published: true
title: Deployment is Unix
---

Over the last 3 years I've worked a lot on [Etsy's deployment
system][deployinator] (we've recently [brought the Open Source version back
into sync with our internal changes][deployinator_post] and are running on the
public version now as well). It's at the core of our development process as
all development is framed in the context of continuously deploying small
changes to the website. And the process of putting in feature flags and always
comitting to master follows from that. Deployinator is a Sinatra/Ruby
application that executes Bash scripts and commands in the background. It has
two buttons - for staging and production - that run the (shell) commands to
execute a list of deployment tasks. The usual tasks include refreshing the git
checkout on the build box, building/minifying JavaScript and CSS, compiling
templates, and rsyncing code to all the web servers (with our [atomic
deploys][atomic_deploys] there is also some symlink flipping involved). But
that's it, it's a very simple concept.

[![deployinator - ruby in the front, bash in the
back](/images/deployinator-ruby-bash.png)][talk]

Of course the overall application has a lot of features. And they keep growing
and changing as we figure out how a growing engineering team is using it. As
we have remediation items coming from PostMortems. And as more teams need and
add more deployment stacks for slightly different applications. The original
version of deployinator had an execution model where all commands where
executed in a streaming manner within an HTTP request. That meant we had to
configure the correct output buffering, had a long running request doing the
work and generally a somewhat confusing scenario where we often weren't sure
what would happen when you close your laptop lid in the middle of a deploy. We
also started to run into problems where the deploy would often break with SSH
broken pipe errors (all commands are run over ssh) in the middle of a run. We
tracked it down to an oddity in TCP behaviour between modern versions of OSX
and Linux.  And we decided that it was time to move the deploy run out of the
HTTP request. We thought about different ways of doing that and prototyped a
couple of things. And then one day while working on one of the prototypes of
the new deployment model I took a step back and realized that I was basically
trying to [reimplement OS process management in Ruby][unixtweet]. And this was
not what I wanted Deployinator to be. Deployinator is **UNIX**. So a
deployment is now done by [forking into a separate process][fork], setting the
process title to the stack and stage name and letting it run. `ps`, `kill`,
`nice` all still work. If you need to log into the deployment server and
figure things out, you can still use the tools you use every day. The rest of
Deployinator also always has been very UNIX inspired. The deployment process
runs commands over ssh and distributes commands to multiple machines via
[dsh][dsh]. All deployment output is written to a log file. The log file is
tailed by a websocket server to present it back to the web application. The
log in the web app shows all output of what the shell commands are doing. If
the commands write to STDERR, Deployinator shows it in red and bubbles it up
to a separate error log. This means you can write your deployment commands in
the well known UNIX style. Infos go to STDOUT, errors go to STDERR. In
addition Deployinator also comes with a command line tool to kick off any
deploy without needing a working web server.

And in my opinion this is how it should be. The actual steps of how your
software gets deployed will always be a little different. You might run a
Rails app instead of a PHP application. You might have a compiled binary that
needs to be shipped or you will have to restart services. You might git pull
on the servers directly instead of rsyncing files over. But there is always
the operating system as the common denominator (or almost always). And by
using that foundation in your tooling you already have a common ground when it
comes to understanding and debugging what your deployment system does. And
there are a lot of existing tools, like rsync, git or ssh which you can reuse
and leverage. There is also a [great response][gh_deploy] by [Corey][corey]
about how the GitHub deployment system works. And the final paragraph there
is really what I love most about it:

> I think people would be underwhelmed by the technology and implementation though. It's just a bit of ruby, UNIX, and HTTP. It's not pushing the boundaries of computing, it just chugs along doing its job so we don't have to.

And it really doesn't have to be complicated. Deployment can start with a
simple shell script. And then you can wrap it in a web frontend. Or an IRC
command.  Or an iPhone app. But at its core it's still manipulating files and
putting them on a computer. Deployment is still UNIX.


[deployinator_post]: https://codeascraft.com/2015/02/20/re-introducing-deployinator-now-as-a-gem/
[deployinator]: https://github.com/etsy/deployinator
[talk]: https://speakerdeck.com/mrtazz/the-road-to-success-is-paved-with-small-improvements?slide=68
[atomic_deploys]: https://codeascraft.com/2013/07/01/atomic-deploys-at-etsy/
[fork]: https://github.com/etsy/deployinator/blob/master/lib/deployinator/app.rb#L183
[unixtweet]: https://twitter.com/mrtazz/statuses/380547968174415872
[gh_deploy]: https://gist.github.com/atmos/6631554
[corey]: https://twitter.com/atmos
[dsh]: https://www.netfort.gr.jp/~dancer/software/dsh.html.en
