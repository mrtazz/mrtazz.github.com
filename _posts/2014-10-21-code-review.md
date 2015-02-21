---
layout: post
published: true
title: Code Reviews Considered Awesome
---

I think by now it is pretty much accepted that collaboration and working
together is a lot better than assigning blame and yelling at each other. In
the field of operating web services and infrastructure we have made the shift
in the last years to improving techniques and simplifying processes, everybody
is modelling infrastructure as code, nobody throws stuff over the wall
anymore, developers are on-call and there is rarely any yelling (at least
that's the ideal most companies strive for).

But I think there is a very undervalued part of that whole situation.  A lot
of operations engineers (at least in my part of the internet) will happily
claim they can't write code or write shitty code (by the way if you still
think you don't write code as an ops engineer go read [Christopher Webber's
great blog post][notacoder] about it). When in reality ops engineers write a
ton of code and I have seen and reviewed amazing apps and tools being knocked
out by people who will happily make excuses for their code whenever you talk
about the awesome thing they wrote. The big advantage of working together is
exchanging knowledge that was previously in siloed domains. It's great that
developers carry pagers and know about deployment and how to write Chef
recipes or Puppet modules. But that also means as an operations engineer you
can tap into a giant stream of software engineering knowledge that developers
have learned, improved and cared about for years. And one of those is code
review.

Code review is a great way to get free learning and feedback about the things
you are working on. It's a way to get someone with a different context to
think about whether your solution to a problem (and that's what basically all
programming, scripting and automation is) makes sense to someone else.  It's
also a way to learn about paradigms, conventions and unknown techniques to
make things better. I'm a software engineer by trade and whenever I start with
something new, work on a new project or try to solve a new problem I try to
seek out code review. I recently worked on our Android app. And while I have
worked with Java and written Android code before, it's been years and was in a
completely different code base. So while I was writing down the code that would
solve the problem I was having, it was very non-idiomatic when it came to our
Android coding style. Once I was done I asked my coworker [Hannah][hannah] if
she could take a look at my code. And she was super excited about it and
immediately jumped on it. And she gave me *tons* of feedback. She showed me
the app loader structure that would make my code much more [DRY][dry] and I
learned a lot about how we approach Android development. And even though she
initially wasn't completely familiar with the problem I was solving and it
definitely took a bit longer for me to actually deploy the code I learned a
ton.

One of the keys here was definitely that she was so excited to review code for
me. And this is where the reviewer comes in. Being asked for a code review
means the person values your opinion and would love to get your feedback on
something that is arguably important to them and trusts you to be able to
improve it. It also means you now have to balance the fine line between not
actually giving useful feedback and overdoing the code review and effectively
blocking their progress. E.g. while it likely doesn't make sense to suggest an
abstract factory pattern for a procedural script of python code, showing
where it could greatly benefit from using functions is a simple way to improve
things. Fundamentally how to give good code reviews is a complicated topic
which I don't want to get into here. But the important part is that this is
something you should be excited about and should let the person asking for the
review know that you are.

Often code reviews are still seen as a slow down, annoying and nothing
that really adds value as you already know your code works. However seeing
code reviews as an opportunity to learn, get another perspective and
ultimatively a way of sharing knowledge is in my opinion way more accurate
when done right (*right* being used lax here as everyone has to define for
themselves what that means). It is a great way to learn about different parts
of your infrastructure's code, new programming paradigms and methods, how to
communicate more effectively and in the end leads to a more resilient
organization and enjoyable work and programming environment.


[notacoder]: http://cwebber.net/blog/2014/09/26/i-am-not-a-coder/
[hannah]: https://twitter.com/hannahmitt
[dry]: http://en.wikipedia.org/wiki/Don't_repeat_yourself
