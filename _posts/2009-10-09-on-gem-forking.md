---
title: On Gem Forking
layout: post
---

![](http://cloud.github.com/downloads/qrush/litanyagainstfear/fork2.JPG)

So, [GitHub has recommended
Gemcutter](http://github.com/blog/515-gem-building-is-defunct) as an
alternative to hosting gems on GitHub. Of course, there seems to be an
outcry, for three main reasons that I can see:

1\) Effort must be made to move documentation and install instructions  
2) Gem forking is not possible with the new site  
3) GitHub gave no warning on this

As for \#1, you’ve got a year to update your user base and get gems off
GitHub. I’d assume that any actively maintained project has figured
Gemcutter out by then. And for \#3, at least we’ve got a new service
that works and can meet the same needs.

Regarding RubyGem forking, I’d like to state that this statement is
**false**. Gemcutter accepts built gems, so you simply need to mimic the
actions that GitHub’s gem builder did: Open your .gemspec up, append
your username to the gem name, and save. `gem build`, `gem push`, and
done.

I think an important distinction must be made here: **gem forking != scm
forking**. GitHub made it easy for anyone to automatically push
modifications to gems, but there’s a bigger picture to think of here.
Hopefully at some point, the changes you made will be brought back into
the mainline gem. As [David Dollar](http://daviddollar.org/) so
eloquently puts it:

<blockquote>

I think the general idea is that gem forks should be a huge special
case. I’m somewhat of the mind personally that making gem forks too
‘easy’ causes a great deal of unnecessary fragmentation in the
community. It seems reasonable to me, that if your project is going to
depend on a gem fork, that the dependency resolution not be automatic,
and your installation instructions can tell the user how to get the
forked dependency.

If a fork is going to be long-term, or a true alternative, it should
probably be reregistered under a new name as a different project.

</blockquote>

Like I stated above, ‘gem forking’ is still supported just because of
the nature of how Gemcutter works. I felt there needed to be a longer
term solution for this in general, since now the community has grown
used to it. This [lively
discussion](http://wiki.github.com/qrush/gemcutter/fork-support) ensued,
and after some deliberation we’re going to use subdomains in order to
solve the problem.

The idea is this: you’ll be able to register your own subdomain on
gemcutter.org, such as `qrush.gemcutter.org`, and we’ll give you a
completely blank index to push to. Hopefully you’ll be able to add
others like the [gem owner](http://gemcutter.org/pages/gem_docs#owner)
system works now. I feel this feature needs to be used in the following
ways:

1\) Subdomain use should be infrequent. It’s for trial/forked gems that
shouldn’t be relied on for production.  
2) Use prerelease versions for development snapshots (version numbers
like 1.0.0pre, `gem install yourgem --prerelease`)  
3) Don’t add someone’s subdomain as a source, unless you can completely
trust anything they toss there. (like, your own for example)  
4) Consider gemcutter.org/rubygems.org as the main, canonical repo that
you can trust.  
5) Start looking into [gem
signing/cert](http://blog.segment7.net/articles/2009/02/04/a-rubygems-github-proposal)
since it’s **the** way we can really trust gems

Of course, none of this subdomains stuff works yet, but it seems like
the best way forward. It’s also spawning new, awesome ideas like
[password protected, private
subdomains](http://github.com/qrush/gemcutter/issues#issue/91). If
you’re interested in contributing to the project, please [fork
away](http://github.com/qrush/gemcutter) or hop in \#gemcutter on
Freenode to see what’s happening.

This is **our** gem host now, let’s make it awesome.
