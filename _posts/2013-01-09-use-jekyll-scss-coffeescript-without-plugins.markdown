---
layout: new
title: Use Jekyll, SCSS, and CoffeeScript without plugins
tags: without-plugins
---

I've been using Jekyll for [almost 4 years now](https://github.com/mojombo/jekyll/commit/23031152359e2f963fbef5908c470dda05493d00), which is long before SCSS and CoffeeScript existed. Since then, there have been a slew of plugins that build on top of Jekyll that provide everything from new "models" to an asset pipeline like Rails. I have a simple rule with how I use Jekyll:

> If I need more than what Jekyll provides by default, I don't use Jekyll.

I'm super happy that people can build on top of Jekyll's "core". Projects like [Octopress](http://octopress.org/), and stories like [Obama raising $250M](http://kylerush.net/blog/meet-the-obama-campaigns-250-million-fundraising-platform/) with Jekyll are awesome for the community. However, if I need something more complex than simply "posts" and "pages" for a static site, I'm not going to try to shoehorn Jekyll into a solution.

If you want to use Coffeescript or SCSS with Jekyll, you don't need to modify Jekyll to get this done. You can just use gems, and rake. My sites use an "assets" directory, which contain all of the .scss and .coffee files, and then some simple Ruby process spawning generates the assets. Here's a gist so you can use it too:

<script src="https://gist.github.com/4496420.js">
</script>

I've written 3 fully fledged Jekyll sites using this method to generate assets, and here's the source to them for you to check out:

* [OpenHack](http://github.com/openhack/openhack.github.com)
* [CoworkBuffalo](http://github.com/coworkbuffalo/coworkbuffalo.github.com)
* [Litany Against Fear](http://github.com/qrush/qrush.github.com)

There's a few downsides to this method that I'd like to put out there before you go ahead and remove your plugins:

* Errors aren't reported too well
* Killing the rake process with ^C leaves some gross error messages
* This doesn't combine multiple assets into one file
* Heavy caching means that CSS refreshes are hard sometimes

If you can live with the above problems, I hope this code helps you out. Let me know if it does!
