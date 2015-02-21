---
layout: post
title: "Setting up workstations with Chef (Newbie Edition)"
published: true
---

I have wanted to reinstall and cleanly set up my iMac at home for some time
now. And since there was a new release of Mac OS X around the corner, it seemed
like the perfect opportunity to do so. All my past setups and reinstalls were
guided by a useful [gist][setupgist] I forked from [Kenneth
Reitz][kennethreitz] some time ago and adopted to my needs. However this time I
wanted to do it a bit differently, I wanted to take this as an opportunity to
dive into configuration management with [Chef][chef]. As I prepared my
configuration I found a lot of things confusing and not so well documented for
a complete newbie. Thus I wanted to share my experience and maybe provide an
overview and easier access into the world of Chef. After all once you have your
setup it is a pretty nice way to keep your workstations' configuration in sync
and have a documented way how you got there.

The setup I am going to describe is based heavily on Joshua Timberman's
[post][jtimberman] about managing Mac OS X workstations with Chef. If you
already know Chef, go read it, it's great. As all my workstations are running
OS X, the steps described are only actually tested on this OS, but should
hopefully apply for any other supported OS as well. And of course the setup
should be installable to the environment of a normal user (no need to wake up
root just because you want to add a plugin to your shell).

However as I am very new to Chef and configuration management, some things may
not be described 100% accurately, so read this post with two big hands of salt
(or two cups of coffee).

### Configu-what?
If you are not familiar with configuration management, you can go read it on
the [Wikipedias][confmanwiki]. But in a nutshell it is the possibility to have
an automated build with one build target which is 'set up the machine
production ready'. As in a classical automated software build, the system knows
what needs to be done to complete the build target and can track what has
already been done. Therefore all steps are idempotent, which means executing a
step multiple times always leads to the same result (and no duplicated
resources).  Therefore it is important that you treat your configuration in the
same way you would treat your automated build: There are no steps executed
outside the system. If you force yourself to use your configuration management
system for every install and configuration you will see how it simplifies your
life, at least when you set up a new machine again. Chef is one implementation
for such a management system (other popular choices are [Puppet][puppet] and
[cfengine][cfengine]). Chef is (mainly) written in Ruby and supports cookbooks
written in Ruby itself or the Chef DSL which we will see in a later example.

### To the Cloud!!
Chef comes in two flavours: [Chef Server][chefserver] and [Chef
Solo][chefsolo]. The main difference here is that with Chef server everything
related to your configuration is managed on a server and machines register on
it to get their configuration and then perform all actions locally
with `chef-client`. Chef Solo on the other hand is basically a client run where
you have to download your configuration manually beforehand. The downloaded
configuration is then used by the executable to set up your machine. So in a
Solo run there is no external resource involved, but there are also some
features which are only available in the server edition. For managing my own
configuration I decided if I am going to learn Chef I might as well do it with
the full stack. However setting up Chef server is a real hassle as many
different technologies are involved and is not really recommended for someone
new to Chef. Fortunately [Opscode][opscode] (the company behind Chef) provides
a so-called 'Hosted Chef' service, which really just means a Chef server in the
cloud. And as it is free up until 5 nodes, it is a great way to get started
with Chef.


### Clients, nodes, knife, cookbook, recipe?
The basic terminology can be a bit confusing (especially as half of the search
results usually link to gourmet sites). So let's try to clear some terminology
right upfront:

- Cookbooks: Basic Chef configuration/distribution unit
- Recipe: Subunit of cookbooks. All basic steps are taken in recipes
- Client: A client which connects to the Chef server, level at which
  certificates are issued
- Node: An actual machine which asks the server for its configuration
- Roles: Collection of cookbooks which can be assigned to nodes
- Knife: Command line client to interact with the Chef server
- Data bags: JSON encoded information which doesn't fit anywhere else to store

This might still be a bit confusing, but let's just start with our configuration
to see how these parts all play together. The big benefit of Chef (I'm sure
it's the same with most of the other systems), which is also a point which is
often discussed as a weakness, is the fact that everything really is Ruby or
json. This means it is source code, which again means we can easily manage it
with an SCM (I will use git in the examples, but it really applies to your
favourite SCM, too). So let's start with creating our configuration repository:

{% highlight bash %}
    $ mkdir chef-repo ; cd chef-repo
    $ git init .
{% endhighlight %}

Now that we have our repository set up, we can start to add cookbooks.
There are in general two ways to get cookbooks into your repository.

- create the files and folder yourself
- knife (the command line client, remember?)

Knife is definitely the better way as you can create cookbook scaffolds, add
cookbooks directly from the community site or use one of the great plugins
(like pulling cookbooks directly from Github). But to get a better
understanding of the cookbook basics, we'll create everything by hand now.

### The first cookbook
As an example cookbook we'll want to install [oh-my-zsh][ohmyzsh] with our own
custom `.zshrc`. Although this is probably not such a common install as `git`
for example, it is a reasonably easy one and a good example for how to
automate steps which would normally be done manually. The steps we want to
automate are:

- download and install oh-my-zsh
- install our custom `.zshrc`

So first of all let's create the basic folder structure:

{% highlight bash %}
    $ mkdir -p cookbooks/oh-my-zsh/recipes
    $ mkdir -p cookbooks/oh-my-zsh/templates/default
    $ touch cookbooks/oh-my-zsh/recipes/default.rb
    $ touch cookbooks/oh-my-zsh/templates/default/dot.zshrc.erb
    $ touch cookbooks/oh-my-zsh/README.rdoc
    $ touch cookbooks/oh-my-zsh/metadata.rb
{% endhighlight %}

The rough knife equivalent (which creates all the possible folders for the
cookbook) would be `knife cookbook create oh-my-zsh -o./cookbooks`. However in
order to get our oh-my-zsh cookbook working, we only need the files and folders
shown above. The `README.rdoc` and `metadata.rb` files are just for metadata
about the cookbook and only the Ruby file is directly parsed by the Chef server
for information. But every cookbook should also contain a README which
explains its purpose in a spoken language (you create README files for all of
your projects, don't you?).

In order to setup the cookbook, first insert your current `.zshrc` into
`oh-my-zsh/templates/default/dot.zshrc.erb`. This makes it available to our
recipes as a template file. Now we want to configure the actual recipe.
Therefore enter the following into `oh-my-zsh/recipes/default.rb`:

{% highlight ruby %}
    script "oh-my-zsh install from github" do
      interpreter "bash"
      url = https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh
      code <<-EOS
        curl -sLf #{url} -o - | sh
        rm #{ENV['HOME']}/.zshrc
      EOS
      not_if { File.directory? "#{ENV['HOME']}/.oh-my-zsh" }
    end
{% endhighlight %}

This just executes the shell script passed to the `code` directive. The used
interpreter is `bash` and the `not_if` directive secures the idempotency of
this step. The script is only executed if the directory `~/.oh-my-zsh` does not
exist. The shell script just contains the usual oh-my-zsh installer and removes
the generic `.zshrc` which is important for the next step. As we want to
install our own config file but don't want to do it everytime, we use the
following Chef block (written to `oh-my-zsh/recipes/default.rb` directly after
the install script):

{% highlight ruby %}
    template "#{ENV['HOME']}/.zshrc" do
      mode   0700
      owner  ENV['USER']
      group  Etc.getgrgid(Process.gid).name
      source "dot.zshrc.erb"
      variables({ :home => ENV['HOME'] })
      not_if { File.exist? "#{ENV['HOME']}/.zshrc" }
    end
{% endhighlight %}

This creates the file given as the template parameter (our zsh config file)
with the given properties. It makes sure the file is owned and only readable by
us, takes the content from the `dot.zshrc.erb` template and passes `variables`
to the renderer. As you might have already seen, templates are just [ERB][erb].
This means we can use the ERB syntax (`<%= var %>`) within a template to insert
dynamic content passed from the recipe.

One additional step, we might want to take, is source `.profile` in our config
file. This is especially useful if you use environment management like
[rvm][rvm], [virtualenv][virtualenv] or [kerl][kerl]. These usually need to be
activated in the shell config. In order to make sure that they are present in
every shell the activation step is written into `.profile`. Therefore we also
want to source it in our zsh config. The `not_if` method here also conserves
the idempotency of the step.

{% highlight ruby %}
    script "source .profile in .zshrc" do
      interpreter "bash"
      code <<-EOS
      echo "source #{ENV['HOME']}/.profile" >> #{ENV['HOME']}/.zshrc
      EOS
      not_if "grep \"source #{ENV['HOME']}/.profile\" #{ENV['HOME']}/.zshrc"
    end
{% endhighlight %}

### The server comes into play

After finishing these steps, we can upload the cookbook to our server.  In
order to be able to do this, the server needs to be set up, so if you haven't
already [sign up][chef-signup] for a free hosted chef. After creating your
organization, put your client and validation certificates in `~/.chef`. I find
this to be a convenient place for all your Chef related configuration, but you
can of course choose another directory (just make sure that you also adapt
subsequent steps in this post accordingly). Now we can upload our cookbook
with:

{% highlight bash %}
    knife cookbook upload oh-my-zsh
{% endhighlight %}

We have a cookbook on the server now, but no node uses it, yet (we also don't
have nodes set up at the moment but bear with me here).  In order to match
nodes to cookbooks Chef employs the concept of 'run lists'.  These are
basically lists of recipes which can be added to a node so that it knows what
to install. As run lists are mostly very similar between nodes of the same
category, we can set up a role for it in Chef. A role is just a specific set of
attributes and a run list which is mapped to a name. As there may be multiple
machines we use as workstations we create a role 'workstation' in the roles
directory of our Chef repository:

{% highlight bash %}
    $ mkdir -p roles
    $ touch roles/workstation.rb
{% endhighlight %}

Again this is just Ruby so we add the following information to
`workstation.rb`:

{% highlight ruby %}
    name "workstation"
    description "development workstations"
    run_list(
      "recipe[oh-my-zsh]"
    )
{% endhighlight %}

Now every node which is assigned the 'workstation' role will know that it has
to install chef `oh-my-zsh` recipe. Let's upload the role to our server:

{% highlight bash %}
    $ knife upload role from file workstation.rb
{% endhighlight %}

In the management web interface (or via `knife`) we can now assign the role
'workstation' to specific nodes. However we first need a client which is
allowed to connect to the server API. Clients and nodes are somewhat the same
in Chef. Theoretically it is possible that a client manages a number of nodes,
but normally every node corresponds to one client. Therefore we create a new
client for our workstation. You can also run `chef-client` on your node and
provide the validator certificate for your organization. If the node does not
yet exist on the server it is created. However this means that you have to have
the validator certificate (which is the ultimate key to your server) on the
node. This might not be a problem for setting up your development machine, but
is bad security in general. So the better way is to create the client and node
on the server and provide the correct credentials (at least read and update)
for the client on the node. One more advantage is that we can now already
assign roles to our nodes (via the 'Roles' menu) and add the 'workstation' role
to the newly created node. All these steps can of course also be accomplished
with `knife`, but I find the web management console easier to start with.  When
all this is done, download the client's certificate and also put it in
`~/.chef`. Theoretically your node is correctly set up already. However Chef
makes the assumption that it is run with privileges. Therefore the default data
directory is in `/etc/chef`. As we want to setup our development machine and
not a server, it makes sense to run `chef-client` as your normal user. In order
to do this, you would now have to make the default directories accessible for
your user. But we can also override the paths used in the client config. I also
keep my paths in `~/.chef` (everything in one place, remember?) so a good
adaption of your `client.rb` might be:

{% highlight ruby %}
    base_dir = "#{ENV['HOME']}/.chef"
    run_path "#{base_dir}/run"
    checksum_path "#{base_dir}/checksum"
    file_cache_path "#{base_dir}/cache"
    file_backup_path "#{base_dir}/backup"
    cache_options({:path => "#{base_dir}/cache/checksums", :skip_expires => true})
{% endhighlight %}

This will make sure only subdirectories of `~/.chef` will be used for
caching, checksums, etc. After these steps there is only one thing to do.

### Sit back and watch

{% highlight bash %}
    $ chef-client -c ~/.chef/client.rb -k ~/.chef/client.pem
{% endhighlight %}

The above command will run the Chef client with the specified config and client
certificate. It will then fetch the cookbooks from the server, determine which
to execute via the nodes run list and run them. If everything went well you now
have oh-my-zsh installed and can go on and add additional cookbooks to your
repository.

### Further reading
You should now be equipped with a basic working setup to create your
configuration with Chef. Play around with new cookbooks and try to force
yourself to do everything system configuration related in terms of cookbooks
and data bags. You'll only learn it by doing it. If you feel comfortable enough
with this basic setup, see the following links for some more sophisticated
possibilities.

- [Managing workstations with Chef][jtimberman]
- [Encrypted data bags][encdatabags]
- [Light Weight Resource Providers(LWRP)][lwrp]



[setupgist]: https://gist.github.com/513101
[chef]: http://opscode.com/chef
[confmanwiki]: http://en.wikipedia.org/wiki/Configuration_Management
[chefserver]: http://wiki.opscode.com/display/chef/Chef+Server
[chefsolo]: http://wiki.opscode.com/display/chef/Chef+Solo
[jtimberman]: http://jtimberman.posterous.com/managing-my-workstations-with-chef
[ohmyzsh]: https://github.com/robbyrussell/oh-my-zsh
[puppet]: http://projects.puppetlabs.com/projects/puppet
[cfengine]: http://cfengine.com
[encdatabags]: http://wiki.opscode.com/display/chef/Encrypted+Data+Bags
[lwrp]: http://wiki.opscode.com/display/chef/Lightweight+Resources+and+Providers+(LWRP)
[kennethreitz]: http://kennethreitz.com
[erb]: http://ruby-doc.org/stdlib/libdoc/erb/rdoc/classes/ERB.html
[rvm]: http://beginrescueend.com/rvm/install/
[virtualenv]: http://pypi.python.org/pypi/virtualenv
[kerl]: https://github.com/spawngrid/kerl
[chef-signup]: http://www.opscode.com/hosted-chef/
[chef-nodes]: http://wiki.opscode.com/display/chef/Nodes
[opscode]: http://www.opscode.com
