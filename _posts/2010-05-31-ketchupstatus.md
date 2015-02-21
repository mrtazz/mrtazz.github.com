---
layout: post
title: Where's ma ketchup?
published: true
---

At our office mondays are special. This is the one day Bobby drives his
good old trailer to the parking lot of the supermarket next to the office and sells
chicken and fries.
As an alternative it is also possible to only get fries and get burgers for the
microwave from the supermarket. But there is still one more important thing: Ketchup.
There are even weekly fights, about which brand is the best (no doubt, Heinz is where
my heart is). Ketchup is a shared resource with us. One person usually buys a bottle
for everybody and when it is empty, the next one is bought by someone else.

The problem is once we are in the supermarket, usually nobody has checked if we still
have ketchup or not. And so the guessing starts. This is why I decided to build an easy
web application where we can set and check the current status of our ketchup supply.
So last weekend I sat down and since I wanted to get into ruby anyway, I built a sinatra
app. It is quite simple and works with a token based API, but it does the job. It is hosted
on [heroku][ketchup_heroku] if you want to check it out. But you can also fork it on
[the githubs][ketchup_github], if you want to improve it or adapt it to something else.


[ketchupstatus_post]: {{ page.url }}
[ketchup_heroku]: http://ketchupstatus.heroku.com
[ketchup_github]: http://github.com/mrtazz/ketchupstatus
