--- 
wordpress_id: 23
title: Pimping the Windows Command Line
wordpress_url: http://www.litanyagainstfear.com/blog/2008/03/20/pimping-the-windows-command-line/
layout: post
category: windows
---

Lately I’ve noticed that a few of my friends have been able to
understand or even be aware of the Windows command line. I have a hard
time myself pulling myself away from Explorer, but sometimes the command
line is just quicker. However, it’s a bit drab by default. Today I’m
going to show you just how you can pimp yours out a bit.<!--more-->

First up is a wonderful tool called Console2, which is an open source
tabbed command line utility. It allows easy configuration of fonts,
colors, the background colors, and even lets you do some really crazy
things if you wanted such as adding a background image. The best feature
by far though are its tabs, which I’m sure if you’ve ever used IE7+ or
Firefox you know how useful they can be for multitasking. Another great
feature is that you can set up plenty of keyboard shortcuts if you’re
into that sort of thing. Check it out here:
<a href="http://sourceforge.net/projects/console/">http://sourceforge.net/projects/console/</a>

Console is great if you want to fire up a specific program all the time,
but I’m used to slamming WinKey + R and typing <strong>cmd</strong> so
much that it’s basically embedded into my muscle memory. So, I was
looking for configuring the default prompt instead of running another
program. Pimping out my command prompt has taken me quite a while to
find the perfect configuration that suits my needs, and it’s constantly
changing as I find new things to add. Here’s what I’ve done to trick
mine out:

I’m using Consolas as my font. I absolutely love Consolas and I find it
much easier to read than Courier New or the default font that the
command window has. Consolas is not available by default and requires a
simple registry hack along with a reboot in order to work. I haven’t had
any issues with it thus far, and I highly recommend following
<a href="http://www.hanselman.com/blog/UsingConsolasAsTheWindowsConsoleFont.aspx">Scott
Hanselman’s wonderful tutorial</a>.

I’m not a Windows command line user at heart, I was raised during my
Computer Science classes to poke around Linux machines. Sadly, certain
commands like <strong>ls -la</strong> come much more naturally to me
than <strong>dir /a</strong>. My solution to this is installing the
<a href="http://gnuwin32.sourceforge.net/packages/coreutils.htm">CoreUtils
for Windows</a> package, which adds most of the core GNU utilities into
the command line. This hack requires installing a package from
SourceForge and adding to the PATH environment variable, which are
covered in
<a href="http://www.askstudent.com/tips/how-to-use-unixlinux-commands-at-the-windows-command-prompt/">this
blog post over at AskStudent.com</a>.

Finally, I’m a huge fan of using the <strong>pushd/popd</strong>
commands: push directory and pop directory. Basically it’s a way to
return to directories instead of having to overuse <strong>cd</strong>
or even remember where you were. Luckily, there’s an easy way to keep
track of the amount of directories that have been pushed onto the stack
by changing the PROMPT environment variable to:

<code>$p$\_$+$g</code>

<a href="http://www.askstudent.com/tips/how-to-use-unixlinux-commands-at-the-windows-command-prompt/">Check
out the AskStudent tutorial</a> to see how to do get to the environment
variables window if you don’t know how. What this does is add + symbols
to the command line window as shown in the screenshot. It’s ridiculously
useful, and now I just wish I could figure out how to automagically
<strong>pushd </strong>and <strong>cd </strong>at the same time.

If you’ve got any questions regarding pimping out your command line or
other great hacks I’m missing out on, let me know!
