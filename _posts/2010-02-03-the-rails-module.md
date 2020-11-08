---
title: The Rails Module (in Rails 3)
layout: post
author: Nick Quaranto
---

So, you may have noticed this in the [Rails 3
Changelog](http://guides.rails.info/3_0_release_notes.html..).

<blockquote>

Railties now deprecates:

`RAILS_ROOT` in favour of `Rails.root`,  
`RAILS_ENV` in favour of `Rails.env`, and  
`RAILS_DEFAULT_LOGGER` in favour of `Rails.logger`.

</blockquote>

Great…but why? Better alternatives have existed for a while in Rails
core (some since 2.1.0), and it’s about damn time you start using them
properly. There’s also some other helpful methods on the `Rails` module
we’ll explore in this post.

Rails.root
----------

This is a big one. Every Rails developer has done
`File.join(RAILS_ROOT, "path", "to", "something")` before. Stop that.
And don’t just replace `RAILS_ROOT` with `Rails.root` either.
`Rails.root` is a
[Pathname](http://ruby-doc.org/stdlib/libdoc/pathname/rdoc/index.html),
which means you can do cool stuff like this:

    <code>$ rails console
    >> Rails.root
    => #<Pathname:/Users/qrush/Dev/ruby/new_app>

    >> Rails.root.join("config", "database.yml")
    => #<Pathname:/Users/qrush/Dev/ruby/new_app/config/database.yml>

    >> _.read
    => "development:\n ...

</code>

Rails.env
---------

Same deal, you’ve probably done something like
`if RAILS_ENV == "production"` in your Rails apps. Stop that too. Oh,
you thought this would just be a `String`?

    <code>$ rails console
    >> Rails.env
    => "development"

    >> Rails.env.class
    => ActiveSupport::StringInquirer
    </code>

[Whaaaat?](http://www.hulu.com/watch/124915/family-guy-back-to-jail#s-p1-sr-i1)
Actually, this is a really neat utility. From
`activesupport/lib/active_support/string_inquirer.rb`:

{% highlight ruby %}
module ActiveSupport
  class StringInquirer < String
    def method_missing(method_name, *arguments)
      if method_name.to_s[-1,1] == "?"
        self == method_name.to_s[0..-2]
      else
        super
      end
    end
  end
end
{% endhighlight %}

Awesome. This lets us do stuff like this in
[Gemcutter](http://github.com/qrush/gemcutter):

{% highlight ruby %}
if Rails.env.development? || Rails.env.test?
  include Vault::FS
else
  include Vault::S3
end
{% endhighlight %}

Rails.logger
------------

This is your favorite `Logger` class, just now without an annoying
constant name of `RAILS_DEFAULT_LOGGER`. Much easier to remember.

    <code>$ rails console
    >> Rails.logger
    => #<ActiveSupport::BufferedLogger:0x21de384 ...

    >> Rails.logger.info "zomg!"
    => "zomg!\n"

    >> File.read("log/development.log")
    => "zomg!\n"
    </code>

Rails.public_path
-----------------

A helpful shortcut to what your `public` assets directory is called,
probably to use with `Rails.root`. (Why this isn’t a `Pathname` is
beyond me, sounds like a good patch to whip up!)

    <code>$ rails console
    >> Rails.public_path
    => "public"
    </code>

Rails.cache
-----------

Now the rabbit hole goes deeper. This is a unified interface to
memory/file/you name it caching stores that can be used with Rails. If
you’ve ever made some sort of caching global variable, like `$memcache`
or `CACHE`, you should [read up
here.](http://guides.rubyonrails.org/caching_with_rails.html)

    <code>$ rails console
    >> Rails.cache
    => #<ActiveSupport::Cache::MemoryStore:0x21e04b8 @data={}>

    >> Rails.cache.write("rush", "limelight")
    => "limelight"

    >> Rails.cache.read("rush")
    => "limelight"

    >> Rails.cache
    => #<ActiveSupport::Cache::MemoryStore:0x21e04b8
         @data={"rush"=>"limelight"}>
    </code>

Rails.application
-----------------

The new `Rails::Application` class encapsulates **a lot** of what was
thrown around in Railties in previous releases of Rails, and really
represents the ultimate embracing of Rack’s modularity. [Yehuda’s
post](http://www.engineyard.com/blog/2010/rails-and-merb-merge-rails-core-part-4-of-6/)
can explain it further, but the important thing is now you can run
multiple `Rails::Application`s in the same process if you need to, and
it’s promoting decoupling even further by starting from the inside of
the framework. Awesome.

    <code>$ rails console
    >> Rails.application
    => #<NewApp::Application:0x13896b0 ...

    >> Rails.application.routes
    => #<ActionDispatch::Routing::RouteSet:0x162877c ...

    >> Rails.application.routes.recognize_path("rails/info/properties")
    => {:controller=>"rails/info", :action=>"properties"}
    </code>

Rails.configuration
-------------------

This gives you global access to all of the configuration data set up in
your `config/application.rb` and various
`config/environments/#{Rails.env}.rb` files, if you should ever need it.

    <code>$ rails console
    >> Rails.configuration
    => #<Rails::Application::Configuration:0x7e1ab0 ...

    >> pp Rails.configuration.middleware
    [ActionDispatch::Static,
     Rack::Lock,
     Rack::Runtime,
     Rails::Rack::Logger,
     ActionDispatch::ShowExceptions,
     ActionDispatch::Callbacks,
     ActionDispatch::Cookies,
     ActionDispatch::Session::CookieStore,
     ActionDispatch::Flash,
     ActionDispatch::Cascade,
     ActionDispatch::ParamsParser,
     Rack::MethodOverride,
     ActionDispatch::Head,
     ActiveRecord::ConnectionAdapters::ConnectionManagement,
     ActiveRecord::QueryCache]
    => nil
    </code>

Wrapup
------

I’m sure some are missing here (like `Rails.version`), but these are the
ones that I think matter most to Rails developers. If something else
should be covered here, let me know!
