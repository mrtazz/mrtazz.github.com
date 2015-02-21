---
layout: post
title: "Getting started with monitoring on the cheap and easy"
published: true
---

This post started out as a writeup of tools and services I use to monitor my
small (currently 3) set of personal servers. However thinking about it, it made
more sense to me to structure it as a small guide on how to get started with
monitoring without having to invest too much time, effort and money. Since I
don't use that at the moment, I won't cover instrumentation and monitoring of
application metrics but go more into general service availabilty and machine
level metrics. The prices I mention are (to my best knowledge) up to date for
the current time, but are of course subject to change.

### My setup
I have a small set of servers which I'm using for basic services. These include
mail server, IMAP, backup MX, [IRC bouncer](http://wiki.znc.in/ZNC) and general
remote shell for running [mutt](http://www.mutt.org/),
[weechat](http://www.weechat.org/), [newsbeuter](http://www.newsbeuter.org/)
and other terminal based applications.  I recently got around to more or less
properly create [cookbooks](https://github.com/mrtazz/cookbooks) for this as I
am running [chef](http://opscode.com) for configuration management. This also
prompted me to finally set up monitoring and alerting for the services I care
about.

### External service monitoring
Servers are not very useful when their services are not accessible from the
outside world. So you want to monitor this from an external source which
usually tries to establish a connection to specified TCP ports. The general
first service to use is [pingdom](http://pingdom.com). They provide a great
service with great statistics. However since I want to monitor more than the
free plan offers (and possibly more than the cheapest paid plan also), I was
looking into an alternative. Since I already have an account at
[zerigo](http://zerigo.com) for some DNS services, I decided to give their
[Watchdog service](http://zerigo.com/watchdog) a try. It's $15 per 3 months and
allows 50 service checks for 10 hosts with checking time down to every 5
minutes. This is more than enough for my needs and comes down to $5 a month.
The only drawback is that they only provide email notifications (which can be
somewhat mitigated with [ifttt](http://ifttt.com) or the mail to text gateway
of your mobile provider) to one user and a not really great statistics
overview. Otherwise it works pretty great.

### Process monitoring
The next step is to monitor the processes which are actually providing those
services. For this I'm running a [Sensu](https://github.com/sensu) instance on
[Heroku](http://heroku.com) in the setup I [described
before](http://unwiredcouch.com/2012/07/31/deploy-sensu-heroku.html). Sensu is
an awesome monitoring framework which provides a lot of flexibility, so it's
definitely worth checking out. Since it runs on two small Heroku instances I
can host the server and API for free which works pretty well. As basic checks
I test for running sendmail, cron and dovecot processes. If the checks fail the
given threshold, an alert is pushed to an IRC channel on my
[grove.io](http://grove.io) organization. Admittingly this is a little bit
overkill since the basic plans for grove.io start at $10, but I like to play
and experiment with chat based interfaces to infrastructure automation and
monitoring. An alternative would be to use [Campfire](http://campfirenow.com)
which is free for a small amount of users. I am also playing with the idea of
having a [Boxcar](http://boxcar.io) handler either for Sensu itself or alerting
to Boxcar from IRC. Boxcar is a pretty sweet service which handles push
notifications to mobile phones and I'm already using it for notifications from
my IRC bouncer and [ifttt.com](http://ifttt.com). And since I'm also running an
instance of [Hubot](http://github.com/github/hubot) (also on a free Heroku
instance) it should be rather trivial to have the bot listen for patterns and
send Boxcar notifications upon match.

### Log processing
Since I don't want to log into several servers to quickly check different
logfiles, I'm sending all of my log data to
[Papertrail](http://papertrailapp.com). They provide an easy endpoint to send
log lines from various systems such as syslog, rsyslog or directly from an
application with an rsyslog handler. Their basic free plan allows for 100MB of
log data per month with a searchable archive of 1 week. This amount should be
enough for a small set of systems with average log data. After that you get 1GB
of log lines in the first stage of paid plans for $7, which is still a decent
trade. The big advantage is that I can now log into a web interface and see
specific log information (for example about chef runs) across all of my
servers.

### Machine level metrics
Additionally I also gather machine level metrics for all of my servers. These
include basic information about CPU and memory usage, disk space and uptime.
All of these metrics are gathered by [collectd](http://collectd.org) and its
various plugins and are sent to [Librato Metrics](http://metrics.librato.com)
for graphing. This is a lot easier and less hassle than managing your own
[Graphite](http://graphite.wikidot.com/) instance. And you only pay for the
metrics you actually send. The data I currently send there are basic metrics
from 2 servers and the number of Sensu check occurrences and it adds up to
something around $5 a month.

### Verdict
This setup gives me (in my opinion) a pretty good monitoring solution for my
personal infrastructure. Since I don't consume a lot of resources for the
services I depend on, I can usually use the free or cheapest plan available.
With the cheapest options it's around $10 a month and even adding grove.io and
paid Papretrail into the mix only brings you to a bit more than $25 a month.
Of course depending heavily on 3rd party services opens a whole new discussion
about [availability](http://whoownsmyavailability.com) which you should be
aware of.

For configuration examples for the services mentioned above, you can check out
my [chef cookbooks](https://github.com/mrtazz/cookbooks). They are mostly run
on FreeBSD but should be somewhat easy to adapt to a different environment.
