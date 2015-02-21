---
layout: post
title: My 5 years of GitHub
---

In early 2009 I was very much not the typical programmer. I had just spent 3
years part time in what was basically systems administration during my
undergrad and 1 year of [trying to connect chemical production plants to
computers][mes]. I had to write code for some of my University assignments and
had done some shell scripting in my spare time before (all the sysadmin stuff
was Windows and there was not a lot of scripting involved there). However in
general I had this idea that systems administration and IT was completely
orthogonal to writing code and a lot of the Software Engineering classes I had
up to the point didn't really spur my interest. They usually featured a lot of
software processes, XML, SOAP, and simple C#.NET Windows applications. Nothing
I was really interested in. But after graduating in 2007 and working full time
I had this feeling every day that something was missing.  I had no idea what I
wanted to do with my computer degree, I loved working with computers but I
felt I had nothing I was really good at. And the nature of my degree was that
it was very practical. Which helped me a lot setting foot and getting started
in the industry, but I felt like I was missing all the theoretical education
you would get in a more traditional university setting. So I decided to go
back to University and get a Masters degree. And I also quit my job and worked
part time as an Embedded Systems developer. Which put me way more out of my
comfort zone than I expected. But it also confronted me with a lot of new
things regarding software development. My first project at work was actually a
web based network sniffer that ran on a microcontroller. So I accidentally
started learning more about web development while working at an Embedded shop.
And another really fortunate accident was that one of our lead engineers -
[Nathan][nathan] - was really into git. And me being a subversion fan at the
time resulted in some really interesting discussions which made me look into
git.

### Enter Twitter, twsh and GitHub
I had signed up for Twitter in mid 2007 while I was writing my Bachelor thesis
and literally had no idea what I was supposed to do with it. In my circle of
coworkers and friends who worked with computers I was most of the time one of
the first to explore new things and thus I didn't know a single person with a
twitter account. I don't even remember how I heard about Twitter in a world
that doesn't have Twitter. But I had [extremely interesting tweets][tweet1]
back then already of course. Fast forward to 2009 my new found interest in
programming from my new job and the programming I had to do for class
assignments in university meant that I was constantly trying out new things
and experimenting with the concepts I learned. So at some point I decided I
wanted to have a twitter command line client and started to write [the twitter
shell][twsh]. It was painful and slow and I had no idea what I was doing. It
was living there in a subversion repository on my Mac mini at home and I
wanted to open source it eventually. I had no idea what that really meant
either. But I had used a lot of open source software and was always fascinated
by the idea that I could just look into how things work.

I first looked into hosting it on Google code but I found it to be ugly and
weird. And a lot of people in my twitter stream were talking about git and
this new thing called GitHub. Since there was nothing really tying me into
subversion, I moved the code over to git and signed up for a GitHub account on
March 28th 2009.

### Brave New World
And it *literally* changed my world. I suddenly found a lot of people who were
doing so many interesting things. And it encouraged me to work on all the
ideas I had floating in my mind but thought were useless or boring. I found
out about the programming communities behind languages like ruby or python.
How to package software to be installed from PyPi. I started thinking about
how to split software into projects as libraries and how to design APIs. I
wrote API wrappers for things like [Instapaper][instapaperlibrary], [a Notifo
library][notifolib] or a tool to import YAML based groceries lists into the
[iPhone Groceries app][groceries] and I also heard about continuous
integration for the first time.

And I was fascinated by the idea of having a build system do all those things
I usually ran commands for automatically. I read up on it and tried to get
[Integrity][integrity] up and running, which seemed to be the most accessible
solution to me at the time. And having worked on the notifo library I wanted
it to push to my phone whenever a build would break or work (yes in general it
was quiet enough for me back then that I wanted all of the notifications). So
the time had come to contribute to an Open Source project and figure out how
to ruby and write unit tests and all of those things. I wrote the notifier,
submitted a pull request and after some feedback and improvements, [Simon][sr]
merged my notifier into master and I was super excited. I was finally able to
have it running on my own CI instance and know the status of my builds with
just a quick glance at my phone:

![integrity pull request merged](/images/integrity-notifo.png)

After that I was less terrified of contributing to Open Source and just trying
out things. I kept perusing the GitHub explore page and found all those
interesting projects. I went into something like an Open Source rampage and
tried to contribute to and open source as much as possible. I even signed up
for [Calendar About Nothing][can] and maintained something like 120 days with
consecutive contributions at some point. And whenever I decided to push a new
project I would meet and engage with more people and learn new things. For
example I wrote the [C++ implementation][plustache] of Mustache templating for
fun and met [Jan][janl] because of that. Which then led to me meeting a lot of
other awesome people in Berlin and around the internet.

And even though twsh never actually got finished and I lost interest in it, I
can definitely say that GitHub has changed my computing life to the amazing.
And I probably wouldn't be where I am now without it.



[integrity]: https://github.com/integrity/integrity
[nathan]: https://github.com/nbraun
[twsh]: https://github.com/mrtazz/twsh
[tweet1]: https://twitter.com/mrtazz/statuses/130573092
[sr]: https://twitter.com/sr
[notifo]: https://twitter.com/notifo
[groceries]: http://sophiestication.com/groceries/
[instapaperlibrary]: https://github.com/mrtazz/InstapaperLibrary
[notifolib]: https://github.com/mrtazz/notifo.py
[instareader]: http://www.unwiredcouch.com/2009/09/19/google-reader-instapaper.html
[plustache]: http://www.unwiredcouch.com/2010/04/21/plustache.html
[janl]: https://twitter.com/janl
[can]: https://github.com/blog/178-it-s-a-calendar-about-nothing
[mes]: http://en.wikipedia.org/wiki/Manufacturing_execution_system
