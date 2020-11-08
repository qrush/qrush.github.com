--- 
wordpress_id: 9
title: "Flash and WPF: A pain in the ass."
wordpress_url: http://www.litanyagainstfear.com/blog/2008/03/09/flash-and-wpf-a-pain-in-the-ass/
layout: post
category: windows
---

I could say this about WPF as a whole, but when things actually work
with WPF it’s so damn smooth I can’t help but feel somewhat
accomplished. Nevertheless, I had a real pain trying to get an embedded
flash player into my WPF app. Why Flash? Why not…dare I say,
Silverlight? Well, I’m building a small app that plays YouTube videos,
so obviously Flash makes sense. First, here’s some lessons I learned
while on this wonderfully frustrating journey:<!--more-->

1.  **For this instance, the <code>WebBrowser</code> control blows.**:
    My first attempt was to add an embed tag into a
    <code>WebBrowser</code> control, which is put inside of a
    <code>WindowsFormsHost</code> control, which is how .NET
    encapsulates pre-3.0 controls. This did not work in the long run
    because first of all, IE throws script errors when you try to play
    YouTube’s .SWF files, and also when resizing the background flashes
    and looks like utter garbage. Don’t get me wrong, it has its uses.
    Just not here.
2.  **Pre-packaged solutions also suck.**: There’s a few flash solutions
    out there that I tried, either got working and
    <a href="http://www.f-in-box.com/dotnet/order.html">had to pay
    ridiculous amounts for</a> (oh look, $1000 off this month!), or
    plainly didn’t work at all. Just don’t use them.
3.  **Google is your only hope.**: Basically any problem I’d come across
    I’d have to google immediately, and answers were sparse. The
    solution I finally ended up with was embedded ActiveX objects, which
    I also had a lot of trouble with, hence this blog post.

So, here’s an overview of what I did, and how you can get it working. A
small note, I’m targeting .NET 3.0.

1.  Reference <code>Interop.ShockwaveFlashObjects</code> and
    <code>AxInterop.ShockwaveFlashObjects</code> in your project. The
    latter assembly may only be the necessary one, but back in the 2.0
    world the forms designer added both. These DLLs will be included in
    the download project, but you can usually find them in the COM tab
    of the Add Reference dialog.
2.  For whatever <code>UserControl</code> or <code>Window</code> you
    want to show this on, set up a <code>WindowsFormsHost </code>object
    as well as a <code>AxShockwaveFlash </code>object. (Make sure to
    include a <code>using AxShockwaveFlashObjects;</code> as well)
3.  In the <code>Loaded</code> event for your
    <code>UserControl</code>/<code>Window</code>, NOT the constructor,
    set up these objects and set the <code>Child</code> property of the
    <code>WindowsFormsHost</code> to the <code>AxShockwaveFlash</code>
    object you made. If you don’t do it in this order, or do this in the
    constructor, ActiveX gets quite angry at you.
4.  Make sure to add the <code>WindowsFormsHost</code> to the
    <code>UserControl</code>/<code>Window</code>…this is one of those
    gotchas that can save you some time.
5.  Now you can set the <code>Movie</code> property of the flash player,
    and call <code>Play()</code> and it should work fine. If not, leave
    a comment and I’ll help you out.

Here’s an example project for you to get started with:

<a title="Flash WPF Example Project" href="http://litanyagainstfear.com/wp-content/uploads/2008/10/freepopcorn.zip"><img src="http://litanyagainstfear.com/wp-content/uploads/2008/03/flashwpf.jpg" alt="Flash WPF pic" /></a>
