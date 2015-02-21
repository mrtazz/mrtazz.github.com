---
layout: post
title: "Deploying Sensu monitoring on Heroku"
published: true
---

### Sensu - trying to unsuck monitoring
Some months ago I wanted to set up monitoring for a handful of servers I use
for personal stuff. As a first solution [Nagios][] came to mind. However for
several reasons I didn't want to set it up and configure it. And I really
didn't want to dedicate an existing server to do monitoring or get a new one
just for that purpose. Around that time I also read about [Sensu][], a new
approach to monitoring, which is a result of Nagios not being a good fit for
the monitoring needs at [Sonian][]. Its technology stack is Ruby, Redis and
AMQP. I immediately thought it should be possible to put this on the [Heroku
Cedar stack][] and run it on an instance there, which would make a nice
solution for monitoring a small number of systems. So I hacked away and with a
lot of help (and patience) from [Sean Porter][], the adaptions to make the
server and API part of Sensu deployable on Heroku are in the new [0.9.6
release][].

### Setting up the Sensu repository
In order to get started and configure your Sensu instance, clone the [example
repository][] from Github.

{% highlight bash %}
    git clone https://github.com/mrtazz/sensu-heroku-example
{% endhighlight %}

The example includes a basic folder layout for running a server or API instance
on Heroku. All configuration files can be dropped in the `config/` folder. They
will be picked up by the process when Sensu starts. The example repo also
includes a basic handler (`bin/showme.rb`), which prints event data to STDOUT.
There are a lot more handlers in the Sensu [community plugins][] repository on
Github. Since handlers are just ruby scripts, you can download the handlers you
want and also put it in the `bin/` directory. Don't forget to add the correct
configuration file for the handler in the `config/` directory also. A great
overview how to configure Sensu can be found on Joe Miller's [blog][] and there
is also an official [install guide][].

### Deployment
In order to deploy Sensu to Heroku, you need to create two apps. One will be
the Sensu API instance and the other one the Sensu server. It doesn't really
matter, which one you start with. The important thing is, that you only need to
add the RabbitMQ and Redis plugins once and can then reuse the settings on the
second instance.

So create the first instance on the cedar stack from within the example repo
and add the plugins:
{% highlight bash %}
    heroku create --stack cedar awesome-sensu-server
    heroku plugins:install redistogo
    heroku plugins:install rabbitmq
    heroku config:add API_PORT=80
{% endhighlight %}
You have to add the `API_PORT` environment variable to the server instance,
since otherwise it will assume it's running the API itself and assign the
instance locale port from the `PORT` environment variable to use as the API
port. After that is done, push the code to Heroku and scale up a worker
process:

{% highlight bash %}
    git push heroku master
    heroku ps:scale app=1
{% endhighlight %}

For the API instance create a new branch in the repo or clone the example repo
into a new location. Then initialize the API:
{% highlight bash %}
    heroku create --stack cedar awesome-sensu-api
    heroku config:add REDISTOGO_URL="value from server instance"
    heroku config:add RABBITMQ_URL="value from server instance"
{% endhighlight %}
Now change the Procfile to start up the API instead of the Sensu server like
this:
{% highlight bash %}
    app: sensu-api -v -c config/config.json -d config/
{% endhighlight %}
Commit the changes and push it to the Heroku app:
{% highlight bash %}
    git push heroku-api master
    heroku ps:scale app=1
{% endhighlight %}

Now all you have to do is set up clients and voila, you have Heroku hosted
monitoring. If you're not yet familiar with setting up clients, I highly
recommend Joe Miller's [blog][] again. He's a strong contributor to Sensu and
has written an abundance of blog posts and tutorials about it. And of course
there is also the [sensu wiki][].

### Further improvements
A definite improvement for plugins and handlers would be to be able to also
read configuration from environment variables. At the moment the way to go is
to add a configuration JSON file in the config folder. This is fine except for
the fact that you'd also have API keys commited to the repo.

And obviously more bugs will probably come up, once more people run Sensu on
Heroku. I've been running a low volume instance for a couple of weeks now and
it works pretty great so far.


[Sensu]: https://github.com/sensu/sensu
[Nagios]: http://nagios.org
[Sonian]: http://www.sonian.com/
[Heroku Cedar stack]: https://devcenter.heroku.com/articles/cedar/
[0.9.6 release]: https://github.com/sensu/sensu/tree/v0.9.6
[example repository]: https://github.com/mrtazz/sensu-heroku-app
[community plugins]: https://github.com/sensu/sensu-community-plugins
[blog]: http://joemiller.me
[install guide]: https://github.com/sensu/sensu/wiki/Install-Guide
[sensu wiki]: https://github.com/sensu/sensu/wiki/
[Sean Porter]: https://twitter.com/portertech

