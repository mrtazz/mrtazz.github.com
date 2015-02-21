---
layout: post
title: "Human Error and Getting off the Hook"
---

I've been interested in the field of human error and system safety for a while
now. My original interest in it got sparked by talking to [John
Allspaw][allspaw] and ultimately reading Sidney Dekker's [The Field Guide to
Understanding Human Error][fieldguide] which gives a very good introduction to
the topic. The book gives a lot of examples of things that have gone bad -
often in aircraft control - and even given the fact that I read most of it on
a 14 hour long flight I can definitely recommend it. I have since then
participated in a book club about the field guide and completed an informal
course about learning how to facilitate a [blameless postmortem][postmortem]
taught by John. This approach of figuring out what happened and why it
happened in a blameless manner makes a lot of sense to me. I have worked in
more traditional places before where incidents weren't investigated in such a
way and I always had the feeling that there was something missing. That the
full story was never really uncovered - not even close. Over time I have
talked to quite a lot of people about the New View, this new way of thinking
about what contributes to incidents and blamelessly investigating them. And
when I talk to people about it who have never heard of this before or are
new to the topic, there is usually one question that comes up really quickly
and is usually something along the lines of:

> But isn't this just a cheap way of getting off the hook?

This is why I decided to write down my thoughts about why this isn't the case
and what the New View is about relating to responsibility and trying to
prevent the same incident from happening again.

First let's look into what we are working with every day. Be it in airtraffic
control and flying airplanes, operating modern trains, working in a hospital
and taking care of patients or keeping a website running. All of those are
complex socio-technical systems. That means the systems as a whole consist of
many many technological parts and humans as operators interact with them. And
they are big and complex enough as so they are in itself intractable for any
person. This means at no point is there a simple and clear plan to follow and
at no point is it possible for anyone to fully describe the system end to end
with all of its interactions. This means anyone working within the system has
to choose carefully between checking every single step for any risk
imaginable and actually getting work done (something that Erik Hollnagel calls
ETTO or Efficiency-Thoroughness-Trade-Off). For example an airplane pilot
might speed up going through the pre take-off checklisting because being
extremely thorough almost certainly means introducing delays or maybe even
missing the plane's flight slot. A doctor maybe only goes through the part of
the patient report that is relevant to the immediate action or surgery they
are about to do because of the huge number of patients they have to take care
of. A software engineer wants to make something faster to provide a better
experience for the user and subsequently brings down the site by exhausting
available resources too fast.

Now these very specific examples might seem like people slacking off and if
they just did all those things according to the rules and regulations,
everything would be fine and nothing can go wrong. And conversely if we fire
the person that caused that deviation from the rules we have a perfectly
simple reason why our complex system broke. This is a very natural
approach to accident investigation. Even Nietzsche talked about it before.

> In the search for a cause of an accident we do tend to stop, in the words of
> Nietzsche, by ‘the first interpretation that explains the unknown in familiar
> terms’ and ‘to use the feeling of pleasure … as our criterion for truth.’
>
><p class="cite">
> &mdash; <cite>Erik Hollnagel, The ETTO Principle: Efficiency-Thoroughness Trade-Off (p. 10)</cite>
></p>

The reality is however that in complex, intractable systems it's impossible to
follow all the rules and attend to work with 100% thoroughness. There is even
a behaviour called ["work-to-rule"][worktorule] that describes the action of
working exactly what the rules describe and thus causing a slowdown that can
even come close to a stop.

So now that we have established that people take
shortcuts and thoroughness tradeoffs all the time, we can also safely assume -
as those actions are likely what makes sense at the time - that other
operators (would) do the same. Bringing us to a point where certain actions
that just before seemed like the cause of trouble are now considered to be a
natural behaviour of people working within the system. And as Sidney Dekker
puts it so aptly:

> Indeed, as soon as you have reason to believe that any other practitioner
> would have done the same thing as the one whose assessments and actions are
> now controversial, you should start looking at the system.
>
><p class="cite">
> &mdash; <cite>Sidney Dekker, The Field Guide to Understanding Human Error (p. 195)</cite>
></p>

But is that really true? Maybe all the others who would have done this very
thing a dozen times before just had better judgement? Maybe they just were
more aware of the situation and noticed that it would be an appropriate
response. Whereas in the failure case the operator just failed to recognize
that now was not the time to do this. It turns out smart people have thought
about this very thing before. The Austrian physicist and philosopher Ernst
Mach came to [this conclusion in 1905][ernstmach]:

> Erkenntnis und Irrtum fließen aus denselben psychischen Quellen; nur der
> Erfolg vermag beide zu scheiden. Der klar erkannte Irrtum ist als Korrektiv
> ebenso erkenntnisfördend wie die positive Erkenntnis.
>
><p class="cite">
> &mdash; <cite>Ernst Mach, Erkenntnis und Irrtum (p. 116)</cite>
></p>

This translates to something like "knowledge and error flow from the same
mental sources; only success can tell one from the other. A clearly recognized
error as a corrective is fostering knowledge as much as positive
realization.". Which makes it clear that the decision of whether or not
something was "the right thing to do" or an error is defined by post-hoc
analysis of the situation. An advantage the operator didn't have in the
situation.

So now what? Our precious theory about the bad apple is gone. Where do we go
from there? Are we not allowed to talk about human actions at all anymore?

Quite the opposite.

Humans are a crucial part of socio-technical systems. But they are what makes
systems safe. As we have said before, our complex systems are in large parts
intractable and thus there is no way we could design a ruleset of things that
we could have a machine execute and everything would be safe. The thousands of
little adjustments, human operators carry out every minute are the pillars of
our system safety. With the little crux that they sometimes also lead to
adversarial outcomes. And this is the part we are interested in. How does
something that is done over and over again seemingly suddenly lead to an
incident? Why does it make sense for a person in that situation to act in the
way they did? After all the basic assumption is that people don't come to work
to do a bad job.

> There is a difference between explaining and excusing human performance.
>
><p class="cite">
> &mdash; <cite>Sidney Dekker, The Field Guide to Understanding Human Error (p. 196)</cite>
></p>

So does the human operator in this New View get off the hook? The answer here
is no. Because thinking about failure and outages in this way means the
practitioner was never on the hook for explaining their behaviour in the first
place. However being part of an incident on the very sharp end of the
situation brings some new responsibilities with it. It means the human is now
the specialist with most of the knowledge about how the system surprised us
and broke down. They know best how they expected the system to react and what
it actually did. They are the foremost authority on what detections they
utilized and what to put in place to realize faster that something is going
wrong. They know which tools they reached for, which they had to improvise,
and which tools they were missing. This means they are very much on the hook.
But on the hook for helping to find ways to make the system safer going
forward.

If this has sparked your interest in the field, my coworker [Ian][indec] has
also recently published a set of resources on the [Etsy Engineering
blog][justculture] to get started with the topic of System Safety, Human Error
and Just Culture.




[fieldguide]: http://amzn.com/0754648265
[etto]: http://amzn.com/B009KOA6LA
[allspaw]: http://www.kitchensoap.com/
[postmortem]: http://codeascraft.com/2012/05/22/blameless-postmortems/
[worktorule]: http://en.wikipedia.org/wiki/Work-to-rule
[justculture]: http://codeascraft.com/2014/07/18/just-culture-resources/
[ernstmach]: https://archive.org/download/erkenntnisundirr00machuoft/erkenntnisundirr00machuoft.pdf
[indec]: https://twitter.com/indec
