---
layout: bit
title: "Context specific dotfiles"
categories:
  - bits
tags:
  - unix
  - dotfiles
  - git
  - vim
  - zsh
---

##[{{page.title}}]({{ page.url }})
I have a [collection][muttfiles] [of][vimfiles] [various][zshfiles]
[dotfiles][dotfiles] which I use to configure the most important tools I use
everyday. Naturally all those are kept in git and shared between all the
machines I work on. The problem is that there might be things I don't want to
store publicly. This might include shell aliases to hostnames, git user emails
I only use at work, etc. I used to manage this by having a different branch
checked out on machines at work and would just merge in master whenever
something changes. However this was super tedious as I had to remember to
switch to the right branch depending on whether I wanted to make public or
private changes. And after changing something I had to remember to switch back
to the correct branch and not accidentally push the private branch to public
GitHub. What it effectively ended up being was a whole bunch of dirty repos on
different machines that were never in sync and partly had duplicate changes
and partly only worked on that box anyways. And whenever I wanted to bring
them back in sync it was a huge pain.  So I decided to adopt a new strategy
for managing context specific dotfiles.

I added a git repo `~/.dotoverrides` to all the machines I work on (or at
least most of them) which contains a `vimrc`, a `zshrc` and so on.  On my work
machines this is pushed to a repo on our internal GitHub Enterprise instance
so I can easily share it between machines. And all my regular dotfiles now
source those override files at the very end.

So in my regular `.vimrc` I have something like this:

{% highlight vim %}
" source overrides configs
if filereadable($HOME."/.dotoverrides/vimrc")
  exec ":source ". $HOME . "/.dotoverrides/vimrc"
endif
{% endhighlight %}

In my `.zshrc` I have this:

{% highlight bash %}
[ -f  ${HOME}/.dotoverrides/zshrc ] && source ${HOME}/.dotoverrides/zshrc
{% endhighlight %}

And in git (only works if you have at least v1.7.10) I've added this stanza:

{% highlight linux-config %}
[include]
  path = ~/.dotoverrides/gitconfig
{% endhighlight %}

Now I can easily share and push/pull my regular dotfiles  in public GitHub and
don't have to pay attention whether or not I'm on the correct branch and if
I'm not accidentally pushing to the wrong remote. Whenever I need to use
different settings on a work machine I just make sure to add it to the
overrides file and have it ready as soon as I open a new shell, run a git
command or open vim again.

So much easier!


[dotfiles]: https://github.com/mrtazz/dotfiles
[vimfiles]: https://github.com/mrtazz/vimfiles
[zshfiles]: https://github.com/mrtazz/zshfiles
[muttfiles]: https://github.com/mrtazz/muttfiles
