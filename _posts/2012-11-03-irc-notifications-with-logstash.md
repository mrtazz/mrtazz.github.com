---
layout: post
title: "IRC notifications with logstash"
published: true
---

I have spent some time in the last weeks to learn more about
[logstash](http://logstash.net/) and used the kind of bad state of my IRC
notifications as the fun side project to get into it. I now have a pretty
useful (well for me) setup which I thought I'd share.

### The IRC setup
My basic setup revolves around using the [ZNC](http://znc.in) bouncer which
keeps me always connected. I still use [weechat](http://www.weechat.org/) in a
remote tmux session most of the time, but like to have the option to switch
clients without losing my connection or backlog. I also use
[Growl](http://growl.info/) pretty heavily in combination with OSX
notification center to alert me of special keywords or all messages in certain
channels. Past solutions included running the IRC client locally with a growl
plugin or remote tail-ing a notification logfile. Those solutions were close to
what I wanted but tied too much to the client, when I really wanted to have
notifications directly from my bouncer. And since znc has a [module to
log](http://wiki.znc.in/Log) all messages to various logfiles, I decided to get
my notifications from there.

### Enter logstash
I had read about logstash before and decided to give it a try for this. I won't
go into detail about installing and running it here, but check out the [getting
started](http://logstash.net/docs/1.1.4/tutorials/getting-started-simple) for a
good introduction.

For the first important step, we need logstash to listen to changes in the
bouncer's logfiles. This is pretty easy and can be accomplished with the
following logstash configuration bits:

{% highlight javascript %}
input {
  file {
    path => "/home/username/.znc/users/zncuser/moddata/log/*"
    type => "znclog"
  }
}
{% endhighlight %}

Per default the log module puts all log files under
`users/youruser/moddata/log/` and creates a logfile per day which is named
after the channel name and date. The logstash input just reads all files that
are in there and adds a type to the captured logs to be able to better identify
them in subsequent filters. The pattern is not really ideal since older
logfiles are not interesting for notifications but are also kept open. So at
the moment I work around that by moving my logfiles to a backup partition every
night, but there might be a better way to do it.

The next step is to remove lines which I'm never interested in for
notifications, like my own messages and JOIN/QUIT messages for example. For
this the logstash `grep` filter definitions are very useful:

{% highlight javascript %}
filter {
  grep {
    type => "znclog"
    match => ["@message", "\[[0-9:]{8}\](.+?)<USERNAME>"]
    negate => true
  }
  grep {
    type => "znclog"
    match => ["@message", "\*\*\* (Quits|Joins|Parts|.+ sets mode: |.+ is now known as)"]
    negate => true
  }
}
{% endhighlight %}

The grep filter is also very useful for another criterion on which I want
notifications, namely for all of my private messages. Since all
channel names per IRC convention have a `#` in the name, we can just assume
that logfiles without that sign are for private messages. It is important to
set `drop => false` here since we don't want grep to drop the log line (which
is default behaviour).

{% highlight javascript %}
grep {
  type => "znclog"
  match => ["@source", "#"]
  add_tag => ["pmnotification"]
  negate => true
  drop => false
}
{% endhighlight %}

This also needs to be added to the filter section and tags all messages coming
from logfiles without a `#` in the name with `"pmnotifcation"`. Now let's go to
the actual parsing of log events. Since there are going to be some repeated
patterns and I wanted to have an easy way to add new ones, I have a 'pattern
library file' which is included in the configuration.

{% highlight javascript %}
NOTIFYME (pizza|cupcakes|fire)
IRCNOTIFY %{DATA}%{NOTIFYME}%{GREEDYDATA}
IRCTIME [0-9:]{8}
IRCCHANNELS (nunagios|chef|food)
{% endhighlight %}

The terms in capital letters can be used as regex placeholders. The interesting
ones are `NOTIFYME/IRCNOTIFY` which are used as a collection of regexes on
which I want to show a notification and `IRCCHANNELS` which are basically the
channel names for which I want notifications for all messages. In order to get
those notifications I set up a set of grok filters.

{% highlight javascript %}
grok {
  match => ["@source", "%{IRCCHANNELS}"]
  add_tag => ["channelnotification"]
  exclude_tags => ["pmnotification"]
  patterns_dir => '/home/username/logstash-patterns'
}
{% endhighlight %}

This grok ruleset grabs all events from the channels based on the `IRCCHANNELS`
match and tags them with the `"channelnotification"` tag. PMs are excluded from
that match because they have already matched.

{% highlight javascript %}
grok {
  pattern => "\[%{IRCTIME:irctime}\](.+?)<%{DATA:ircsender}>%{GREEDYDATA:ircmessage}"
  tags => ["channelnotification"]
  patterns_dir => '/home/username/logstash-patterns'
}
grok {
  pattern => "\[%{IRCTIME:irctime}\](.+?)<%{DATA:ircsender}>%{GREEDYDATA:ircmessage}"
  tags => ["pmnotification"]
  patterns_dir => '/home/username/logstash-patterns'
}
{% endhighlight %}

These rulesets extract the timestamp, sender and message data for the
notifications into separate fields so they are easily accessible later on. I
have the same ruleset for channel notifications and private messages, because I
didn't find a way to match any tag (the `tags` setting requires an event to
match all given tags) so I couldn't combine them into one rule. Though this
seems like something that should be fixable.

{% highlight javascript %}
grok {
  pattern => "\[%{IRCTIME:irctime}\](.+?)<%{DATA:ircsender}>%{IRCNOTIFY:ircmessage}"
  add_tag => ["notification"]
  exclude_tags => ["pmnotification"]
  patterns_dir => '/home/username/logstash-patterns'
}
{% endhighlight %}

And finally the last pattern ruleset matches the regexes that are defined for all
events and parses them into the fields mentioned before. Notice that all
rulesets include a `patterns_dir` section which points to the folder with the
regex defintions file described above.

The last part of the logstash ruleset is defining an output for the
notifications. For a while I just appended them to a logfile and tail-ed that
from my laptop over ssh. This worked ok, but I had problems with duplicate
notifications when restarting the polling script and wasn't really happy with
this solution. And since I already had Redis running on that host, I thought
I'd give that a try.

{% highlight javascript %}
output {
  redis {
    host => 'localhost'
    data_type => 'list'
    key => 'notifications'
    tags => ["pmnotification"]
    password => 'secret'
  }
  redis {
    host => 'localhost'
    data_type => 'list'
    key => 'notifications'
    tags => ["channelnotification"]
    password => 'secret'
  }
  redis {
    host => 'localhost'
    data_type => 'list'
    key => 'notifications'
    tags => ["notification"]
    password => 'secret'
  }
}
{% endhighlight %}

The output config basically just says the for every type of notification log
event, append it to a Redis list with the name `'notifications'` on
the instance running on localhost.

### The client side
The last part now is actually getting the notifications into growl on the OSX
side of things. For this I have Growl setup to forward everything to
notification center and run the following script on my Mac:

{% highlight python %}
import sys
import gntp
import json
import redis
import gntp.notifier

r = redis.StrictRedis(host='ircserver',
                      port=6379, db=0,
                      password="secret"
)
app = "irc-growl"

while 1:
    key, logline = r.blpop("notifications")
    try:
        log = json.loads(logline)
    except Exception as e:
        title = "Failure loading logline: " + str(logline)
        message = "error({0})".format(e)
        gntp.notifier.mini(message, applicationName=app, title=title)
        continue

    try:
        channel = "-".join(log["@source"].split("/")[-1].split("_")[1:-1])
    except Exception as e:
        title = "Failure parsing channel name in: " + str(log["@source"])
        message = "error({0})".format(e)
        gntp.notifier.mini(message, applicationName=app, title=title)
        continue

    try:
        title = ("%s in %s" % (log["@fields"]["ircsender"][0],
                  channel.encode("utf-8")))
    except Exception as e:
        title = "Failure parsing ircsender in: " + str(log)
        message = "error({0})".format(e)
        print title
        print message
        gntp.notifier.mini(message, applicationName=app, title=title)
        continue

    message = (log["@fields"]["ircmessage"][0]).encode("utf-8")
    gntp.notifier.mini(message, applicationName=app, title=title)
{% endhighlight %}

This uses the python gntp library to talk to Growl and the redis client to talk
to Redis. Specifically for the Redis connection I use `blpop`, which pops an
element (in our case a notification) from the list and if there is none waits
for the next one to come in. For every notification it parses out the
timestamp, channel, sender and message from the fields I set in the logstash
grok rules, formats it nicely, sends it to growl and then gets the next one or
waits for new notifications to come in.

## Verdict
There are still some improvements I want to make. Mostly around moving the old
log files or only reading the newest one. And improving the script so it
survives network disconnects and possibly run it under launchd. Also if I'm not
running the script to pull notifications, they are piling up in Redis at the
moment. So next time I connect, I get an abundance of new notifications.
Notification center batches them nicely to not litter the whole screen and only
the last 20 are in the sidebar. So it's not really a problem, but I thought
about running a cron to prune the list to a maximum of 20 notifications or so.

I now have a setup where I get my notifications directly from the bouncer logs
and can display them on any (OSX) host which has the script set up. It should
also be fairly simple to adapt this to other notification display systems. The
setup is no longer bound to which IRC client I use or whether or not I
constantly have it running on a server. Plus the alerting keywords and channels
are easily extended because I only have to add patterns to the library file
and not touch the config itself.

