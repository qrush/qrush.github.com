--- 
wordpress_id: 185
title: Calculating Age in Rails
wordpress_url: http://litanyagainstfear.com/?p=185
layout: post
---

You’d think that this would be easy, but for some reason it wasn’t, at
least for me. Let’s say you keep track of a User’s birthday with a date
field. Great! Let’s show the user’s age.

{% highlight ruby %}
def age
  Date.today.year - birthday.year
end{% endhighlight %}

Done! Right? WRONG. Worked fine for some users, until one of my
coworkers asked me…hey, you’re not 21 yet…how’d you magically gain a
year on your profile? Crap. Obviously this will work for everyone’s
whose birthday is BEFORE <code>Date.today</code>, but not after.

So, we need a more exact method of calculating the age:  
{% highlight ruby %}
def age
  ((Date.today - birthday) / 365).floor
end{% endhighlight %}

So what this method does instead is use the <code>Rational</code> value
given by subtracting two <code>Date</code>s, and divides it by the
number of days in a year. Since that number is still a
<code>Rational</code>, calling <code>floor</code> on it will round it
down to the nearest <code>Fixnum</code>, giving us a (slightly more)
precise account of this user’s age. <strong>EDIT: But this is still
inaccurate!</strong>

Yeah, I thought it was easy too. Perhaps this shows that I need to test
my app a little more thoroughly! It also would be nice to save the age
in an instance level variable, but right now I don’t use the age more
than once on a page so it doesn’t matter.

<strong>EDIT</strong>: It looks like my method is still not sufficient
for leap years! The comments have posted MANY great solutions. My method
is fine for a general estimation, but the comments have many solutions
for that deal with greater accuracy. I’m honestly not sure which is
best, so choose carefully when you’re developing your app.
