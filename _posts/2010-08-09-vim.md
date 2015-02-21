---
layout: post
title: Chipping in on Textmate to Vim switching
published: true
---

There was some buzz lately about people considering [Vim][2] as their main
editor and especially going from [Textmate][3] to Vim. I've tried several times to
go the other way and leave vim as my editor of choice in favour of Textmate. It
never worked.

### Magic does not come easy
I think there is one big mistake a lot of people do when switching. They expect to start
vim and instantly have their fingers dance over the keyboard and coworkers
being stunned by the awesome magic. But as with everything you learn, this is
not the case. It is a long (think years long and not weeks long) way to even
come close to this dance. I started fighting with vim in 2004 and think I have
mastered the first 10% now. In my opinion Yehuda Katz was right when he [said][4]
that you should really start in insert mode. It turns out, that you can really
use Vim (especially [MacVim][5]) like any other editor. From there on, if you
force yourself to learn and use a new command everyday, you will see
significant speed (and magic) improvements quite fast.

### Bundle configuration with pathogen
Another topic which is very important (and powerful) is bundle support.
Textmate's bundles are split into different type in Vim. There are plugins,
compiler, syntax and some more folders which can be used for configuration. If
you do it the old way, it is really cumbersome. You have to copy new scripts
into the according folders in your `~/.vim` folder. You have to check that
nothing gets overwritten, and after some time you will lose track of what
plugins you have installed.

Fortunately, Tim Pope wrote the great [pathogen plugin][6]. You only have to
put it into a folder called autoload and enter 3 lines into your `~/.vimrc` and
your done. Now you can create a new folder under `~/.vim/bundle` for each
plugin you want to install. Pathogen will automatically load the plugin for
you. You can even go further and put your configuration into [git][7].
If you add all of your plugins as git submodule, you can easily update them and
have your configuration in sync on all your machines. It's that easy.

### You mentioned Textmate?
Right. I mentioned that I tried several times to switch to Textmate, as all the
smart OSX users seem to use it. And if all the smart people use it, it must be
awesome right?

After using Textmate for some time, I suffered the same symptoms all the Vim
switchers were talking about. Coding was slow, I had to think how I do this and
that much to often, shortcuts are way to complicated and I tried to find a Vim
mode for Textmate, that worked for me. I was trying to instantly unleash the
magic. And that just doesn't work.
While I think Textmate is (one of) the best pure OSX editors and I hope that
there will be a version 2 someday, I am still incredibly more productive with
Vim. So Textmate is more of a fun to use tool, which I use when I am in the
mood.

But there is still this emacs thing out there, I hear. And it also wants to be
learned.

[1]: {{ page.url }}
[2]: http://www.vim.org
[3]: http://macromates.com
[4]: http://yehudakatz.com/2010/07/29/everyone-who-tried-to-convince-me-to-use-vim-was-wrong/
[5]: http://code.google.com/p/macvim/
[6]: http://github.com/tpope/vim-pathogen
[7]: http://git-scm.com
