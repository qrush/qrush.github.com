---
title: Load Rails conditionally with Rack
layout: post
---

One of the great things about [Gemcutter](http://gemcutter.org) is that
it’s using [Sinatra](http://www.sinatrarb.com) via [Rails
Metal](http://guides.rubyonrails.org/rails_on_rack.html) to serve up the
gems. Recently, I had to run some long migrations (\~10 minutes) and I’m
sure we’ll have some more in the future. Since the gem server is
decoupled from the Rails app, some clever Rack loading now allows us to
continue to serve gems even when we’re down for maintenance.

The `config.ru` now looks like this:

{% highlight ruby %}
if ENV['MAINTENANCE_MODE']
  require File.join('vendor', 'bundler_gems', 'environment')
  require File.join('config', 'environment')
 
  use Rack::Static,
    :urls => ["/index.html",
              "/favicon.ico",
              "/images",
              "/stylesheets"],
    :root => "public/maintenance"
  use Hostess
  use Rack::Maintenance,
    :file => File.join('public', 'maintenance', 'index.html')
  run Sinatra::Application
else
  require 'thin'
 
  run Rack::Adapter::Rails.new(:environment => ENV['RAILS_ENV'])
end
{% endhighlight %}

So now, Heroku treats Gemcutter as a Rack app, and not a pure Rails app.
Normally, it’ll just use Thin and boot up Rails as normal with
`Rack::Adapter::Rails`. `script/server` still works as normal, too, but
I’ll probably use [shotgun](http://github.com/rtomayko/shotgun) instead.

The fun begins when you run `MAINTENANCE_MODE=on rackup`. The following
happens from there:

-   Load up the
    [Bundler](http://litanyagainstfear.com/blog/2009/10/14/gem-bundler-is-the-future/)
    environment that has all of our gems, and some of Rails’ magic.
-   Fire up
    [Rack::Static](http://rack.rubyforge.org/doc/classes/Rack/Static.html)
    to serve static assets like the images and CSS to make the site look
    nice
-   Use the
    [Hostess](http://github.com/qrush/gemcutter/blob/master/app/metal/hostess.rb),
    Gemcutter’s Sinatra gem server to continue to serve gems
-   All other requests are then caught by David Dollar’s
    [rack-maintenance](http://github.com/ddollar/rack-maintenance) that
    simply serves up a static HTML page.
-   Use `Sinatra::Application` since we need some sort of endpoint.

And boom, we have a read-only site that continues to serve gems. Rack
rules.

![](http://6.media.tumblr.com/tumblr_kobz0jtHsN1qzln4lo1_400.jpg)
