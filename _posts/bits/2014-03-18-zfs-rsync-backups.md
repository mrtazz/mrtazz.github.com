---
layout: bit
title: "Backups with rsync and zfs"
categories:
  - bits
tags:
  - freebsd
  - zfs
  - encryption
  - unix
  - rsync
  - backup
---

##[{{page.title}}]({{ page.url }})

As I [mentioned before][uncloud] I'm running my own backup on a server that is
running in my apartment. I didn't really talk a lot about how this works,
other than it is running on a HP Microserver with an encrypted ZFS RAID. So I
wanted to also quickly jot down how the backup works. This is only set up for
a single user right now because I'm the only one using it.

For me a backup has two important parts:

- Have data in a different location
- Be able to restore data from the past

The time sensitivity of those two properties are pretty different for me. For
example I have chosen for myself that I'm happy with only being able to
restore deleted data from the last day. So if I create something and delete it
5 hours later, I'm ok with not being able to recover it. On the other hand I'm
very aware of the fact that my mailserver can disappear at any given time:

<blockquote class="twitter-tweet" lang="en"><p>that moment when you want to make dinner and your mailserver disappears</p>&mdash; Daniel Schauenberg (@mrtazz) <a href="https://twitter.com/mrtazz/statuses/411689583370592256">December 14, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

This is why I want to copy data to a remote location as often as possible
(which for me means about every 15 minutes). And my setup is heavily based
around those ideas. The core of the backup system is ZFS and a separate file
system for each machine I want to backup. In order to have the ability to go
back in time I use [zfs snapshots][snapshots]. Every night the following
script runs on my backup server and creates a snapshot for the day:

{% highlight bash %}
#!/bin/sh
# simple script to snapshot locations on a ZFS backup pool

timestamp=`date +%Y-%m-%d-%H:%M:%S`
for volume in $(ls /backup); do
  echo "Creating snapshot for ${volume} at date ${timestamp}"
  /sbin/zfs snapshot backup/${volume}@${timestamp}
done
{% endhighlight %}

And to make sure that I really do have snapshots I have this simple nagios
script to tell me if the snapshotting worked last night.

{% highlight bash %}
#!/bin/sh

# nagios script to check age of backup snapshots

YESTERDAY=`date -v-1d +%Y-%m-%d`
EXITCODE=0

for backup in $(ls /backup); do
  zfs list -t snapshot | grep ${backup} | grep -q ${YESTERDAY}
  if [ $? != 0 ]; then
    echo "Snapshot of ${backup} missing for ${YESTERDAY}."
    EXITCODE=2
  fi
done

if [ ${EXITCODE} == 0 ]; then
  echo "All backup volumes were snapshotted on ${YESTERDAY}."
fi

exit ${EXITCODE}

{% endhighlight %}

And this check (which runs on all my servers because I have zpools everywhere)
to tell me about the disk health of the backup zpool:

{% highlight bash %}
#!/bin/sh

# check for zpool health
ZPOOL=`which zpool`
EXITSTATUS=0
IFS=$'\n'

for line in $(${ZPOOL} list -o name,health | grep -v NAME | grep -v ONLINE)
do
  echo $line
  EXITSTATUS=2
done

if [ $EXITSTATUS == 0 ]; then
  echo "All pools are healthy."
fi

exit $EXITSTATUS
{% endhighlight %}

With this setup in place I can simply copy files into the file system that
belongs to that machine and it will get snapshotted every night. And what's an
awesome tool to copy data? That's right, [rsync][rsync].

My backup script runs once every 15 minutes and looks like this:

{% highlight bash %}
#!/bin/sh
#
# Backup script to pull in changes from remote hosts
#
for backup in $(ls /backup); do
  grep -q ${backup} ~/.backupexcludes
  if [ $? != 0 ]; then
    /usr/local/bin/rsync -e 'ssh -o BatchMode=yes -o ConnectTimeout=10' \
--archive --delete --timeout=5 ${backup}:. /backup/${backup}/
  fi
done
{% endhighlight %}

This allows me to have machines that I used to backup but are no longer online
in an excludes list. That way rsync (and ssh) doesn't hang or error for
something that doesn't need to be backed up anymore anyways. And in case a
machine is unavailable or disappears the timeout settings in that script make
sure it just gets skipped and retried on the next run.

I'm pretty happy with the setup, my backup server pulls in data from all my
servers on the internet and stores it (forever?). It is chef'd for the most
part (though there is always more to automate) and is pretty simple in my
opinion. The backup situation for my laptop is not ideal yet, as I manually
back it up by running rsync. I want to set the backup server up to also serve
some of the backup filesystems as Timemachine targets, so I can just use
Timemachine on my laptop and have it automatically run the backups.


But in the meantime I can add a new backup with this one weird trick:

{% highlight bash %}
zfs create /backup/newhost && chown -R mrtazz:mrtazz /backup/newhost
{% endhighlight %}

[uncloud]: http://www.unwiredcouch.com/2013/10/30/uncloud-your-life.html
[snapshots]: http://docs.oracle.com/cd/E19253-01/819-5461/gbcya/index.html
[rsync]: http://rsync.samba.org

