---
layout: bit
title: "Shared layout for project pages in Jekyll"
categories:
  - bits
tags:
  - git
  - submodules
  - github
  - jekyll
  - documentation
---

##[{{page.title}}]({{ page.url }})
I use [Jekyll][jekyll] a lot, especially for [my website][unwiredcouch]. And I
quite like it a lot. I also write and open source the occasional software
every now and then, which usually happens on [my GitHub profile][github]. And
thankfully GitHub makes it [dead easy][pages] to generate a nice looking page
for your project. I've used this feature for a long time now and have used a
bunch of their awesome provided themes. However since I also host my site on
GitHub Pages and thus all my projects are automatically available under a sub
path there named after the project name.

However last week I decided that I wanted to have them all be in a layout
similar to my website so the whole page doesn't change just because you click
on a link on my [projects page][projects]. But I also wanted to keep the code
for the pages in the respective repo so it's all in one place while at the
same time I didn't want to copy the layout into each repository.

Thankfully there is trick you can use with GitHub Pages. If you add git
submodules to your repository they are gettiing [pulled in][submodules]
automatically on page build. So I created a [shared repository][layouts] to
hold the template I wanted for my projects. And now all I have to do to get a
project page with the correct layout is:

* `git checkout gh-pages`
* `git submodule add https://github.com/mrtazz/jekyll-layouts.git _layouts`
* copy the `README.md` of my project to `index.md` and add the jekyll
   frontmatter:

```
---
layout: project
title: project name
---
```

* add a `_config.yml` and fill out the following values:

```
gaugesid: tracking code for the gaug.es gauge
projecturl: github url for the ribbon in the upper right corner
basesite: base URL to get the CSS from
markdown: kramdown
```

* `git push`

The only dependency now is that the CSS comes from my main website. Which I'm
fine with and is actually a feature because if I ever change something there I
want the project pages to reflect that change also. The other downside is that
if I change the project layout repository I will have to update the reference
in all the project repositories. Which should be fairly straightforward with
some automation and is at least better than copying files around and
committing them to each repository.


[jekyll]: http://jekyllrb.com/
[projects]: http://www.unwiredcouch.com/projects.html
[layouts]: https://github.com/mrtazz/jekyll-layouts
[unwiredcouch]: http://unwiredcouch.com
[github]: https://github.com/mrtazz
[pages]: https://pages.github.com/
[submodules]: https://help.github.com/articles/using-submodules-with-pages
