---
layout: bit
title: "Mirror GitHub repositories in pure shell"
categories:
  - bits
tags:
  - unix
  - backup
  - git
  - github
  - shell
  - freebsd
  - zfs
---

As I have [written before][uncloud] I have slowly started to move my data out
of cloud services where applicable. One part of that was setting up my own
backup server at home based on [FreeBSD, zfs and rsync][backup]. One part I
consider important data but didn't have on there was my (Open Source) code I
host on GitHub. This also wasn't ever a priority as the code is public anyways
so it wasn't a privacy issue for me, and I also trust GitHub to run backups so
I wasn't overly concerned about my data vanishing. Still I wanted to have my
own backup of things.

So I started to look into how people mirror their repositories for backups,
speed, availability and other things. There exist quite a lot of solutions out
there which are mostly written in Ruby or Python. While this is fine and I
would encourage you to look into those, I didn't want to deal with installing
pip to install some Python script or installing yet another gem just for
something that can be accomplished with a couple of lines of shell. So I wrote
my own set of scripts in Bourne shell (one of the default installed shells in
FreeBSD) so I could just cron them up on my backup box.

First I needed a way to get a list of all my repositories. Thankfully GitHub
has a [pretty great API][ghapi] so I can just get a list of all my
repositories and their git clone URLs:

{% highlight bash %}
#!/bin/sh

# Usage:
# github_repo_list.sh mrtazz [34345k34j3k4b2jk3]
#
# get a list of all public repos for a user
if [ -z $1 ]; then
  echo "Usage:"
  echo "github_repo_list.sh USERNAME [TOKEN]"
  exit 1
fi

if [ ! -z $2 ]; then
  TOKEN="&access_token=${2}"
fi

CURL=$(which curl)
if [ -z ${CURL} ]; then
  # fall back to /usr/local/bin/curl
  CURL="/usr/local/bin/curl"
fi

BASEURL="https://api.github.com/users/${1}/repos?type=owner${TOKEN}"
count=1

while [ ${count} -gt 0 ]; do

  lines=$(${CURL} "${BASEURL}&page=${count}" -s | grep git_url | cut -d" " -f6 | sed -e "s/[\",]//g")

  # stop if we don't get any more content. A bit hacky but I don't want to
  # parse HTTP header data to figure out the last page
  if [ "${lines}" == "" ]; then
    count=0
  else
    for line in ${lines}; do echo ${line} ; done
    count=`expr $count + 1`
  fi

done
{% endhighlight %}

This script takes a username and an optional access token and retrieves the
public list of repositories for that user. It then outputs the git clone URLs
one per line so it's easily stored in a text file or fed into other scripts.
There are some minor inefficiencies and missing features in there as it curls
one more time than needed to the GitHub API to figure out if there are more
results and it also only supports public repositories as I don't have private
ones at the moment. However changing the URL to call if I ever want to mirror
private repositories is relatively easy and I don't care that much about the
extra curl as this script is not gonna be run very frequently.

This now gives me a list of all repositories on my account I want to mirror.
The next step is actually mirroring them. For that I wrote a script that looks
like this:

{% highlight bash %}
#!/bin/sh

# take a list of git clone urls on STDIN and clone them if they don't exist.

if [ -z $1 ]; then
  echo "Usage:"
  echo "github_repo_sync.sh directory"
  exit 1
fi

GIT=$(which git)

if [ -z ${GIT} ]; then
  # if git is not in path fall back to /usr/local
  if [ -f /usr/local/bin/git ]; then
    GIT="/usr/local/bin/git"
  else
    echo "You need to have git installed."
    exit 1
  fi
fi

# switch to archive directory
cd $1

while read line; do
  directory=$(echo "${line}" | cut -d "/" -f 5)

  if [ ! -d ${directory} ]; then
    ${GIT} clone --mirror ${line}
  else
    cd ${directory}
    ${GIT} fetch -p origin
    cd ..
  fi

done

{% endhighlight %}

This script checks for each entry in a list of git clone URLs passed in via
STDIN and if the directory already exists it fetches changes and if not clones
it into the given directory. The mirroring commands reflect the instructions
in this [GitHub guide][mirrorgit].

Now to tie those two together I just set up two cron entries to run those two
commands:

{% highlight bash %}
0 20 * * * ~/bin/github_repo_list.sh mrtazz 0f6 > /backup/github/github_repo_list.txt
0 21 * * * ~/bin/github_repo_sync.sh /backup/github < /backup/github/github_repo_list.txt
{% endhighlight %}

The first cron entry fetches the list of repositories and sticks them into a
text file. The second one runs an hour later and actually syncs all the
changes. I set it up to sync into the zfs pool that gets snapshotted every
night anyways (as described [here][backup]) so I get that for free. I'm not
super happy with running this on a cron as there could be a smarter solution
that checks for changes via the API and marks repositories as dirty, but this
is the simplest thing that could work and way less work than interacting more
with the API. In addition I would love to exclude forks from the backup since
I don't really care about backing those up. But I'll leave this for iteration
2.

I track changes to the script in my [bin folder repository on GitHub][bin], so
if you're interested in tracking changes to this setup, follow it there.


[uncloud]: http://www.unwiredcouch.com/2013/10/30/uncloud-your-life.html
[backup]: http://www.unwiredcouch.com/bits/2014/03/18/zfs-rsync-backups.html
[ghapi]: https://developer.github.com/v3/
[mirrorgit]: https://help.github.com/articles/duplicating-a-repository
[bin]: https://github.com/mrtazz/bin


