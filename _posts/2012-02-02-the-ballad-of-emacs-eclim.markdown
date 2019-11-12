---
title: The Ballad of Emacs-Eclim
layout: post
published: true
categories: [emacs, eclim]
---

I've been using [emacs-eclim] for java development fulltime for well
over two years now, and I've been a co-maintainer for a couple of
months, but I've never really written anything about it. I guess part
of it is because I'm way too lazy to do a big write-up explaining all
the moving parts and how it relates to [eclim] and [eclipse]. You
know, a _proper blog post_.

Like most java programmers, I was a long-time [eclipse] victim. If
you're not from the java world yourself, it might be hard to grasp the
love/hate relationship that all java programmers have with this
IDE. Or hate/hate relationship, as it may be. Sure, some people turn
to [NetBeans] or [IntelliJ], but you can't get away from the fact that
eclipse has in essence steamrolled all over the java IDE
space. Eclipse is _everywhere_.

Incidentally, 'steamrolled' is an appropriate word here, as it
suggests huge mass and inertia bordering on inevitability, both of
which are also properties of eclipse itself. It takes forever to start
up, and if you actually manage to kick off a complex build operation,
it's impossible to interrupt it.

[Emacs] was my escape. But the story is kind of complicated. You see,
there was this bunch of enterprising [vim] hackers that realized that
even though their editor was the greatest thing since sliced bread, it
wasn't really that good at handling large java codebases, something
which eclipse is atually pretty competent at. So they wrote a plugin
for eclipse that turned it into a kind of server, and a vim extension
that acted as a client, and then they could edit the code in vim,
while leveraging the strengths of eclipse, such as command completion,
compilation, and refactoring. And they called it [eclim].

![My setup is obscure](/media/images/obscure-setup.jpg)

Then another [enterprising fellow][senny] ported the client libraries
to [emacs], and the result was [emacs-eclim]. It was a bit rough at
first, but it is improving steadily. In the past year, we've gotten
[auto-complete-mode] support, inline error highlighting, improved
handling of import statements and numerous bug fixes. Right now I'm
working on making calls to the `eclimd` backend asynchronous, to make
editing more responsive. It kind of works, but right now it's ugly.

I actually have no idea how many users we have.  There are
[116 people watching][watchers] the project on Github right now, and
bug reports are being filed every now and then, so I'd like to think
that at least _someone_ finds it worthwhile.

[vim]:http://www.vim.org/
[intellij]:http://www.jetbrains.com/idea/
[eclipse]:http://eclipse.org
[netbeans]:http://netbeans.org/
[emacs-eclim]:http://github.com/senny/emacs-eclim
[senny]:http://github.com/senny
[eclim]:http://eclim.org/
[emacs]:http://www.gnu.org/software/emacs/
[watchers]:https://github.com/senny/emacs-eclim/watchers
[auto-complete-mode]:http://cx4a.org/software/auto-complete/
