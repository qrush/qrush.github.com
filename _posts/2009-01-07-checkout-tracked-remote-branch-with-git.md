--- 
wordpress_id: 396
title: Checkout tracked remote branch with Git
wordpress_url: http://litanyagainstfear.com/?p=396
layout: post
category: git
---

I frequently need to do this when setting up or syncing my various
machines, and I seem to forget the command all the time. So let’s say
you’ve got more than one branch on your remote, and you want to bring it
down into your local repository as well:

![](http://gitready.com/images/branches.png)

Viewing information on the remote should look something like this:

{% highlight text %}
$ git remote show origin
  * remote origin
    URL: *************
    Remote branch merged with 'git pull' 
      while on branch master
        master
      Tracked remote branches
        haml master
{% endhighlight %}

Luckily, the command syntax for this is quite simple:

`git checkout --track -b <local branch> <remote>/<tracked branch>`

So in my case, I used this command:

`git checkout --track -b haml origin/haml`

You can also use a simpler version:

`git checkout -t origin/haml`
