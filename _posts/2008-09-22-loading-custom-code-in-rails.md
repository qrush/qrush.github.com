--- 
wordpress_id: 155
title: Loading custom code in Rails
wordpress_url: http://litanyagainstfear.com/?p=155
layout: post
category: ruby
---

This is a question I’ve seen asked (including by myself) in
\#rubyonrails on Freenode quite a few times, and I figured I’d settle it
once and for all. There’s a few different ways to get custom code loaded
into your Rails app. The first solution to this is understanding how
code gets loaded with Ruby in the first place, which I usually get
confused with. Let’s do a little recap about your different options that
Ruby and Rails provides:

-   <strong><a href="http://www.ruby-doc.org/core/classes/Kernel.html#M005966">load</a>:
    </strong>Loads and executes the Ruby program in the file
    <em>filename</em>.
-   <strong><a href="http://www.ruby-doc.org/core/classes/Kernel.html#M005967">require</a>:
    </strong>Ruby tries to load the library<em></em>, returning `true`
    if successful.
-   <strong><a href="http://wiki.rubyonrails.org/rails/pages/RequireDependency">require_dependency</a>:
    </strong>Reloads source files on each request when in development
    mode, so changes are reflected on the next request.
-   <strong>require_or_load: </strong>There doesn’t seem to be much
    documentation on this, but it doesn’t seem as safe and
    <a href="http://github.com/rails/rails/tree/master/activesupport/CHANGELOG#L1123">it
    may result in your code being loaded twice</a>. Review the code
    <a href="http://github.com/rails/rails/tree/master/activesupport/lib/active_support/dependencies.rb#L228-264">here</a>
    before you use it.

So, the most ideal keyword to use is
<strong>require_dependency</strong>, since it will reload code during
development mode when you make changes. Otherwise, you’ll have to
constantly restart your development server/console, and that just sucks.
Plus, it’ll work perfectly in production mode and only load the files
once.

So, where’s the best place to put the files with your custom code? Well,
there’s a few folders that are on the Rails load path in the first
place: <strong>app, lib, vendor and mock</strong> <strong>paths</strong>
(<a href="http://github.com/rails/rails/tree/master/railties/lib/initializer.rb#L607-609">source</a>)
you need to add different folders to the load path, that’s more than
possible. In your config/environment.rb, add whatever folder you want in
the config.load_paths variable. For instance,

{% highlight ruby %}config.load_paths += %W( custom ){% endhighlight %}

This will load the custom directory (RAILS_ROOT/custom) to the load path
so you can use those files. The problem with files you put in these
directories is that they may be in the load path, but you’ll have to
require the custom files you want in each class that you’ll want to use
them. The solution to this is to get the file required for the entire
Rails environment, which is a lot easier than you’d think.

Let’s say we want to load some extensions to String for your app. Being
a forward thinking developer, you’ve created a new folder in lib, called
core_ext, where you can put other ruby files in the future if you need
to. So in lib/core_ext/string.rb you’ve dumped for example:

{% highlight ruby %}
class String
  def slugify
    self.gsub(/[^a-z0-9]+/i, '-').chomp('-')
  end
end
{% endhighlight %}

The folder config/initializers contains files that run once when your
Rails environment is getting set up. Create a new file in that folder
and this will run through your custom folder and make sure that the
files are required properly.

{% highlight ruby %}
module CoreExtensions
  def require_core_ext
    Dir["#{RAILS_ROOT}/lib/core_ext/*.rb"].each do |f|
      require_dependency f
    end
  end
end
Object.instance_eval { include CoreExtensions }
{% endhighlight %}

So now, you can call <code>require_core_ext</code> in whatever class you
want, and it will reload all of your custom code if you’re in
development mode or if you’re in production, it will only load your
custom classes when the file is first loaded. Now you can call
<code>String\#slugify</code> all you want, and if you make changes to
the method in lib/core_ext it will be reflected when you refresh the
page.

If you’ve got any other examples of how you bring in custom code, let me
know, as I’d love to find out.
