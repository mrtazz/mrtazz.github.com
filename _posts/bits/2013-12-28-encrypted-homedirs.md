---
layout: bit
title: "Creating Encrypted Home Directories in FreeBSD"
categories:
  - bits
tags:
  - freebsd
  - zfs
  - encryption
  - unix
---

##[{{page.title}}]({{ page.url }})
I run FreeBSD with ZFS on all my servers and I generally want to have my home
directories encrypted. Since ZFS native encryption is not yet in FreeBSD, I
create two ZFS filesystems, which are then encrypted with [GELI
encryption][geli] and build a new ZFS pool. This pool is then used as my home
directory. In order to simplify this, I have a shell script that takes the
username and size as input and creates keys and all partitions as well as the
zpool.

{% highlight bash linenos %}
#!/bin/sh

USERHOME=$1
SIZE=$2

zfs create -omountpoint=/encrypted tank/encrypted
zfs create tank/encrypted/keys
zfs create -omountpoint=none tank/encrypted/zvols
zfs create -ocompression=on tank/encrypted/zvols/${USERHOME}
zfs create -V ${SIZE}G tank/encrypted/zvols/${USERHOME}/disk0
zfs create -V ${SIZE}G tank/encrypted/zvols/${USERHOME}/disk1

zfs create tank/encrypted/keys/${USERHOME}
dd if=/dev/random of=/encrypted/keys/${USERHOME}/disk0 bs=64 count=1
dd if=/dev/random of=/encrypted/keys/${USERHOME}/disk1 bs=64 count=1
geli init -s 4096 -K /encrypted/keys/${USERHOME}/disk0 \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk0
geli init -s 4096 -K /encrypted/keys/${USERHOME}/disk1 \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk1

geli attach -k /encrypted/keys/${USERHOME}/disk0 \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk0
geli attach -k /encrypted/keys/${USERHOME}/disk1 \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk1

zpool create ${USERHOME}-home raidz \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk0.eli \
/dev/zvol/tank/encrypted/zvols/${USERHOME}/disk1.eli
{% endhighlight %}

I try to keep the script updated on [GitHub][script].


[geli]: http://www.freebsd.org/doc/handbook/disks-encrypting.html
[script]: https://github.com/mrtazz/bin/blob/master/create_encrypted_zfs_home.sh
