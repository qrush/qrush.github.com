---
title: Gem Bundler is the Future
layout: post
---

[![](http://farm1.static.flickr.com/59/206628748_8f2594a16b_m.jpg)](http://www.flickr.com/photos/mrcam/206628748/)

If you haven’t checked out [Yehuda Katz’s](http://twitter.com/wycats)
and [Carl Lerche’s](http://twitter.com/carllerche) gem bundler yet, [now
is the time](http://github.com/wycats/bundler). This project replaces
the horribly broken dependency resolution in Rails and what we all know
and use as `config.gem`’s. Ever seen <code>can’t activate $gemname
($gemversion = runtime)</code>? Or maybe my favorite, when it can’t even
figure out what gem can’t be activated. If so, read on, bundler’s about
to make your life a lot easier.

Yehuda has a great roundup of what can be done (and has been done with
the bundler) [on his
blog](http://yehudakatz.com/2009/07/08/rails-bundling-revisited/). This
is going to be a part of Rails 3, so you’re going to have to run into
this sooner or later. I was getting frustrated with managing gem
dependencies in [Gemcutter](http://github.com/qrush/gemcutter), so now
it’s bundled up and ready as a decent example of the bundler in action.

**Enter the bundler**

I’m using the edge gem built from their repo, and I suggest you do as
well. Hopefully soon they’ll push it to that [new gem hosting
site](http://gemcutter.org) I’ve heard so much about.

    git clone git://github.com/wycats/bundler
    cd bundler
    sudo rake install

**The Gemfile**

Now we’re ready to make a `Gemfile`. This is basically a specification
of what gems should be included in your app. Here’s Gemcutter’s:

{% highlight ruby %}
clear_sources
bundle_path "vendor/bundler_gems"

source "http://gemcutter.org"
source "http://gems.github.com"

gem "rails", "2.3.4"

gem "clearance"
gem "will_paginate"
gem "sinatra"
gem "xml-simple"
gem "gchartrb", :require_as => "google_chart"
gem "ddollar-pacecar", "1.1.6", :require_as => "pacecar"
gem "net-scp"

only :test do
  gem "shoulda"
  gem "factory_girl"
  gem "webrat"
  gem "cucumber", "0.3.101"
  gem "rr"
  gem "redgreen"
  gem "fakeweb"
  gem "rack-test", :require_as => "rack/test"
end

only :production do
  gem "rack-cache",        :require_as => "rack/cache"
  gem "aws-s3",            :require_as => "aws/s3"
  gem "ambethia-smtp-tls", :require_as => "smtp-tls"
  gem "memcache-client",   :require_as => "memcache"
end
{% endhighlight %}

Some notes here:

-   By default the gem host is `gems.rubyforge.org`, so I’ve cleared the
    sources to use gemcutter.org instead.
-   You can easily specify gems for a given environment and multiple
    gems with `only` and pass it a symbol for the environment name.
    Thank goodness.
-   The default bundle path is `vendor/gems`. This won’t work for Rails
    since it assumes way too much about this directory, so we switch it
    to `vendor/bundler_gems`. **Note:** Yehuda has told me that this
    will work as normal once Rails 3 is closer/done.

**Bundle up!**

Now we’re ready to run `gem bundle`. This pulls down the gems from the
given sources along with any dependencies. This basically creates a
virtual RubyGems environment right inside of your `vendor` directory.
(I’m cutting some out since the log is so long)

    $ gem bundle
    Calculating dependencies...
    Updating source: http://gems.github.com
    Updating source: http://gemcutter.org
    Downloading actionmailer-2.3.4.gem
    Downloading actionpack-2.3.4.gem
    ...
    Downloading xml-simple-1.0.12.gem
    Installing rr (0.10.4)
    Installing fakeweb (1.2.6)
    ...
    Installing nokogiri (1.3.3)
    Building native extensions.  This could take a while...
    Done.

So now in your `vendor/bundler_gems` directory, we’ve got the `.gem`
files pulled down in `cache`, unpacked in the `gems` directory, and the
gem specifications are unloaded into `specifications`. Bundler also
makes its own `environment.rb` for loading the dependencies.

    vendor
    |-- bundler_gems
    |   |-- cache
    |   |   |-- actionmailer-2.3.4.gem
    |   |   |-- actionpack-2.3.4.gem
    |   |   |-- ...
    |   |   `-- xml-simple-1.0.12.gem
    |   |-- doc
    |   |-- environment.rb
    |   |-- gems
    |   |   |-- actionmailer-2.3.4
    |   |   |-- actionpack-2.3.4
    |   |   |-- ...
    |   |   `-- xml-simple-1.0.12
    |   `-- specifications
    |       |-- actionmailer-2.3.4.gemspec
    |       |-- actionpack-2.3.4.gemspec
    |       |-- ...
    |       `-- xml-simple-1.0.12.gemspec
    `-- plugins

Bundler will also dump gem executables in your `Rails.root/bin`
directory. This means you can then use `bin/rake`, for example. Running
`rake` as normal should still work though. As for your version control,
it’s recommended to check in the `.gem`’s only, then run `gem bundle` to
unpack/install them. This goes both for new developers and getting code
deployed.

**Loading the Environment**

Now the issue is to load up the bundled environment instead of the
system installed one. Start by creating a `config/preinitializer.rb`,
which is loaded first before `config/environment.rb`:

{% highlight ruby %}
require "#{File.dirname(__FILE__)}/../vendor/bundler_gems/environment"
{% endhighlight %}

Then, in each `config/environments/*.rb` file:

{% highlight ruby %}
Bundler.require_env RAILS_ENV
{% endhighlight %}

This basically does a `require` for every gem listed in your Gemfile and
their associated dependencies. That should be it! Your app should
(hopefully) boot and now you should run your tests to ensure your
application is still working right.

If you want to see this all in action [clone
Gemcutter](http://github.com/qrush/gemcutter) and follow the
[contribution guidelines for getting up and
running](http://wiki.github.com/qrush/gemcutter/contribution-guidelines).

**Issues**

So, I had a few roadblocks with the bundler, and I don’t think it would
be fair to not mention them.

-   The documentation sucks. I’m hoping this will improve before Rails 3
    is ready (whenever that is). Maybe a Rails guide would be
    appropriate, and I’ll definitely help start it.
-   Gemcutter’s on Heroku, so it’s necessary to check in a lot of the
    vendored code (in fact, all of the development/production
    dependencies). New contributors just have to run `gem bundle -u` to
    get the test dependencies.
-   In my staging environment I had to use
    `Bundler.require_env "production"`. Pretty self-explanatory but I
    missed it at first.
-   Shoulda macros just stopped working, since it assumes the location
    of gems in `vendor/gems` or `vendor/plugins`. I had to include this
    in `test/test_helper.rb` to make it happy:

{% highlight ruby %}
Shoulda.autoload_macros(Rails.root, "vendor/bundler_gems/gems/*")
{% endhighlight %}

**Wrapup**

This guide went over how to use Bundler today, with a Rails 2.3.4 app.
According to Yehuda, this eventually will be packaged in Rails 3, so the
commands will be baked into Rails…so something like `script/bundle`. The
nice thing is that you can use the bundler with any Ruby project, so
this is good to know in general.

The bundler is really the future of gem dependency management. If you’re
sick of fighting with `config.gem`’s or are starting a new app it would
be well worth your time to start looking at it. If you’re having trouble
with the bundler (or success stories!) feel free to comment here or hop
in `#carlhuda` on `irc.freenode.net`. Check it out on
[GitHub](http://github.com/wycats/bundler) if you haven’t yet.
