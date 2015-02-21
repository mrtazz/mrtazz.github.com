---
layout: bit
title: "Productive VIM with templates"
categories:
  - bits
tags:
  - unix
  - vim
  - productivity
  - email
---

##[{{page.title}}]({{ page.url }})

I basically exist inside of vim all day. I write code in there, I write emails
in VIM via [mutt][mutt], I take notes with it and I write all my blog posts in
VIM. I think it's clear that improving the way I work with VIM helps in a
variety of scenarios. Over time I also noticed that I often start out with the
same basic file structure and then fill it with content. For example jekyll
blog posts always have the same header, meeting notes always have the same
structure and I use a template to reply to recruiter emails in times where I'm
not looking for a job (a trick I learned from [Kate Matsudaira][katemats] in
one of her [great blog posts][people-are-lazy] about productivity).

In the coding world VIM provides a great built-in functionality for that which
is called ["skeleton files'][skeleton]. This is a great way to always have a
good to go version of C source or header files, Makefiles or RPM spec files.
However this is all based on filetypes (or rather file endings) and since I
write most of my notes and all my blog posts in [Markdown][markdown] for
example and they all have the same file ending this doesn't help me much for
having different templates. So I started to look around for VIM functionality
or plugins that would just let me load templates from a specific location and
maybe expand some variables (as I for example like to have the date auto
inserted into meeting notes). I didn't want a full fledged templating engine,
although I could certainly have installed and wrapped the [Mustache
implementation written in VimL][vmustache] to do that for me. But I wanted to
keep it simple and apparently that solution didn't exist yet.

This is why I wrote a VIM plugin called [vim-stencil][vim-stencil]. It's a
handful of lines of VimL and it does exactly 2 things:

- Load a template from a specified location
- Expand some variables (currently even only one: the date)

So now with a simple call to `:Stencil` in VIM I can choose a template for the
type of file I'm editing (yes it supports tab completion) and load that into
my buffer. I even get the current date for free in templates where I choose to
have it. No fuzz, no complicated setup. But a small thing that increases my
productivity a lot.


[mutt]: http://www.mutt.org
[vim-stencil]: https://github.com/mrtazz/vim-stencil
[people-are-lazy]: http://katemats.com/people-are-lazy/
[katemats]: https://twitter.com/katemats
[skeleton]: http://vimdoc.sourceforge.net/htmldoc/autocmd.html#skeleton
[markdown]: http://daringfireball.net/projects/markdown/
[vmustache]: https://github.com/tobyS/vmustache
