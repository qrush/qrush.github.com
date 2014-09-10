---
title: Dynos are done
layout: new
tags: dokku
---

I'm tired of having to pay a premium to keep more than one process alive. Dynos are done. DONE!

<img class="center" src="/images/done.gif" width="100%" />

It's time for something new. That new thing is [Dokku](http://progrium.com/blog/2013/06/19/dokku-the-smallest-paas-implementation-youve-ever-seen/). It's the same workflow you are most likely used to: `git push` to deploy. Follow [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-the-dokku-one-click-digitalocean-image-to-run-a-ruby-on-rails-app). In a nutshell you:

* Launch a droplet (cloudspeak for VPS)
* Configure your SSH key to push to the box
* Setup PostgreSQL
* Deploy your app via `git push`

Great! But your app might need more than just a database, unicorn, and nginx. Here's a few helpful notes while you're moving an app onto Dokku/DO: 

### Enable backups on the droplet

This increases the price a little. Peace of mind is worth it.

### Bring in memcached

Another bonus: it's free! Just install [dokku-memcached](https://github.com/jezdez/dokku-memcached-plugin) and you're set.

### Need a www redirect?

You might hate `www` subdomains too. Redirect them with [dokku-redirects](https://github.com/Mordred/dokku-redirects-plugin).

### Shell in

How do you find your logs? They're in a docker container, so it's not fully obvious how to find them. Dokku provides a way to get them, though:

`ssh root@example.com dokku logs yourappname`

If you want to poke around the filesystem for other reasons, or run commands:

`ssh root@example.com dokku run yourappname bash`

I'm sure these could be aliases too you could set up per app. This is begging to be a gem.

### Restarting sucks

Right now, I don't have my dokku setup to restart when the box restarts. This sucks, but there's a [dokku-supervisord](https://github.com/statianzo/dokku-supervisord) plugin for dealing with this.

### You're a sysadmin again

Obviously, that was the premium you were paying. However, I don't mind it that much. Plus, docker/dokku has been pretty fun and enjoyable to work with, and you still get that sweet `git push` experience.

<img class="center" src="/images/noidea.gif" width="100%" />

If this helped you out, I'd appreciate if you signed up for DigitalOcean with [my referral link](https://www.digitalocean.com/?refcode=2d1532006d25). Thanks.
