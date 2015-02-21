---
layout: post
title: plustache
published: true
---

Some time ago I discovered [@defunkt's][@defunkt] logic less
ruby templating system [mustache][mustache]. I instantly
liked it because of its simplicity, independence from any frameworks
and admittingly also because of the name. After having seen many
implementations in [python][pystache],
[JavaScript][mustache.js] and even [Erlang][mustache.erl] and
[node.js][mustache.node] I decided to also port it to a new language.
Since I am doing some C++ at work at the moment and I wanted to deepen
my knowledge in some non-work related projects anyway, the decision was
practically made. So I fired up my trusty
[text editor][macvim] and hacked away.
After some weeks of creating the build system, deciding how to do regular
expressions and unit tests, basic mustache tags, true/false and inverted sections
are working, as well as basic HTML esacping.

The results can be seen on
[github][plustache] and at the moment I am working on
getting to a point where I am satisfied enough with the code to tag it v0.1.0.
I am excited to port mustache to a more static language and challenge the
difficulties of still keeping it simple. And I am even more excited about how this
mustache thing will evolve.

[plustache_post]: {{ page.url }}
[@defunkt]: http://twitter.com/defunkt
[mustache]: http://mustache.github.com
[pystache]: http://github.com/defunkt/pystache
[mustache.js]: http://github.com/janl/mustache.js
[mustache.erl]: http://github.com/mojombo/mustache.erl
[mustache.node]: http://github.com/raycmorgan/Mu
[macvim]: http://code.google.com/p/macvim/
[plustache]: http://github.com/mrtazz/plustache
