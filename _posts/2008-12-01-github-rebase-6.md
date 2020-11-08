--- 
wordpress_id: 377
title: "GitHub Rebase #6"
wordpress_url: http://litanyagainstfear.com/?p=377
layout: post
category: git
---

The sixth edition of my column is live:
<a href="http://github.com/blog/246-github-rebase-6"><span>http://<span>github</span>.com/blog/246~~<span
style="text-align:left;">span\>github</span></span>~~<span>rebase</span>–6</span></a>

I’m slowly improving the parsing process. For the last edition I broke
out all of the work the app does into rake tasks, and then the Rails
site is just for viewing the data. Basically there’s a few steps:

1.  Download ATOM feeds (This week, 1400. Last week, 1200).
2.  Check that all of the downloads went ok. (Bad gateway errors happen
    to good people)
3.  Parse the files, making sure that events are unique and
    <span style="text-decoration: line-through;">users</span> forkers
    are logged.

Surprisingly I’m relying less and less on my app to get the column
together, which is good. Huge kudos to the GitHubbers for creating the
Recently Created page for each language. This helps me diversify the
column while avoiding a lot of data crunching on my end.

Next week I definitely want to parse the entire month’s worth of events
to really see the trends come out. Luckily their atom feed goes back
4000-5000 pages (of 30 events each) or this wouldn’t be able to happen.
What this will mean is that I’ll actually have to optimize the download
and parsing as much as possible. Stay tuned!
