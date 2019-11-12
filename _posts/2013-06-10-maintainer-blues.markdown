---
layout: post
title: "Maintainer blues"
date: 2013-06-10 16:16
comments: true
categories: [eclim]
---

I've been maintaining [emacs-eclim][ee] for a couple of years now. I
like it, since that means I can push my commits to the repo with
minimal fuss. And for a long period of time, that was all that
mattered, since there weren't a lot of other users around.

But that has changed. Over the past few months, we've been getting a
stead stream of [Github issues][issues] and pull requests. There's
even been some traffic over at our [google group]. It seems that
people are getting interested.

That's good.

But here's the thing: emacs-eclim was always just a tool to me;
something I kept hacking on to solve those particular problems I
encountered in my day to day work. So a lot of these feature requests
are things I cannot relate to. Support for PHP? Android development?
Ant? I dunno. These are things I have absolutely no motivation to
spend my free time on. They might be problems, but they're not _my_
problems.

That's bad.

Sure I'd like to achieve feature parity with the [main eclim][eclim]
project (right now there are a lot of commands we don't support). It
would be cool to have better Ruby and Python support. Running unit
tests from inside emacs is probably a useful feature. But I probably
won't be the guy to implement this.

[ee]:https://github.com/senny/emacs-eclim
[issues]:https://github.com/senny/emacs-eclim/issues?direction=desc&page=1&sort=created&state=open
[google group]:https://groups.google.com/forum/?fromgroups#!forum/emacs-eclim
[eclim]:http://eclim.org
