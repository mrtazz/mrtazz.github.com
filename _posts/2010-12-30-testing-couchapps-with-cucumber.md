---
layout: post
title: "Testing couchapps with cucumber"
published: true
---

### Couchapps overview
[Couchapps][2] are a great way to build web apps hosted directly on a
CouchDB. This is due to the integrated HTTP server, so if you can
fit your application into the constraints of HTML/CSS/JavaScript, you
get the storage (almost) for free. The heart of couchapps is [evently][3],
a JavaScript framework simplifying the development of event based web
applications. The development process is accompanied by the
[couchapp python script][4], which maps a certain directory structure
to the evently application layout. This makes it easy to develop the source
code, which is normally stored as attachments in the design document, in your
favourite editor.  However this difference in how the application is developed
and how it is deployed, makes it a bit more difficult to automatically test the
application.

### Enter cucumber
Fortunately the ruby world has provided us with a great tool for
acceptance testing web applications: [Cucumber][5]. Cucumber is a
testing framework, which encourages [BDD][6] style development. It
features different drivers for (headless) browser testing and supports
an easy, natural language like, syntax for creating tests (scenarios as they
are called in BDD world). If you don't use it already, give it a try, it is
really great. However to fully embrace an automated testing approach we need
some helpers to do additional work, for example create and destroy the
testing environments.

### We have the technology, we can make him stronger
The first problem was the CouchDB native authentication db used by couchapps to
profit from the already existing user management. Fortunately there is [a
way][7] to change the db used for authentication to an arbitrary one. The next
nice-to-have is an easy setup for choosing databases for different environments
like tests. Fortunately rails already provides a clean setup for this, which we
can copy. Now we only have to bundle a [simple CouchDB library][8] and some
helper methods and we are ready to go.  This is what [couchapp-cucumber][9] is
about. I bundled all these steps as a simple cucumber drop-in. I'm sure it can
be improved in a lot of ways.

### So fork it. Hack away. And happy testing.


[1]: {{ page.url }}
[2]: http://couchapp.org
[3]: http://couchapp.org/page/evently
[4]: https://github.com/couchapp/couchapp
[5]: http://cukes.info
[6]: http://en.wikipedia.org/wiki/Behavior_Driven_Development
[7]: http://lenaherrmann.net/2010/04/29/security-in-couchdb-changing-the-authentication-db
[8]: https://gist.github.com/738128
[9]: https://github.com/mrtazz/couchapp-cucumber
