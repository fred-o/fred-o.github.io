---
layout: post
title: There's nothing like this
published: false
---

I've been meaning to write something about [emacs-eclim][eeclim] for a
while now, but I don't know what to say or where to start. This post
may turn out a bit rambling and incoherent, but please bear with
me. There is a point coming. I hope.

## The problem with Eclipse 

Actually, there are many problems with [Eclipse][eclipse]. As anyone
who has used it for a non-trivial project can till you, it is a source
of frustration like no other. For example:

+ Maven integration is dodgy at best. Neither the
[m2eclipse][m2eclipse] nor [AIM][aim] plugin is particularly good at
it. I suspect this is partly because the Eclipse project model differs
from the Maven Way and they have to settle for some kind of lowest
common denominator of functionality. Apparently NetBeans is better at
this, but I haven't really tried it.  
+ Eclipse is really childish when it comes to other processes updating
'its' source files, throwing up dialog boxes ('Do you really want to
load these changes?') and showing compilation errors when, in fact,
there are none. Again, NetBeans seems to handle this better.
+ The code editing experience is... adequate at best. The main problem
(apart from inexplicable UI freezes) is that it is too
mouse-centric. Sure, there are Emacs keybindings available, but don't
been fooled: this is not emacs, not by a long shot.
+ Plugin management is a pain, even with the recent 3.4 and 3.5
releases that aimed to solve this problem. 

## The good parts

However bad eclipse is, there are actually some tasks you want to keep
using it for. The refactoring tools are pretty good. The debugger is
great. Also, the code navigation ('Find Type', 'Find Refereces', etc.)
is nice even though the UI isn't.

## Eclim and Emacs-eclim 

Turns out I wasn't the only one fed up with Eclipse. 

My current Java coding environment is a Frankensteinesque combination of:

* An Eclipse instance running eclimd. I use this for refactoring and
  debugging, but I rarely edit my code in it.
* An Emacs instance where I do all my hacking. I have a pretty
  advanced emacs installation, running:

* Emacs-eclim for code completion, navigation, showing compilation
problems, etc.
* [Company-mode][company] with an emacs-eclim backend that provides
  handy pop-up menus for auto-completion.
* [Auto-complete][ac], which also provides some auto-completion
  features. It should be possible to get it to integrate it directly
  with emacs-eclim, but I haven't got it working (yet).
+ [yasnippet][yas] for code snippet expansion. This plugin is great; I
  have a big custom list of expansions for all kinds of things: for
  loops, getters and setters, comments, basic class file skeletons,
  JUnit test cases, etc.
* A bunch of custom maven integration elisp functions.

I won't lie to you; this setup is pretty fragile and it regularly
breaks in unexpected and/or amusing ways. But the thing is, every time
something goes wrong, I can dig into the elisp that caused it, and
usually I'm able to fix it or at least create a workaround. And every
time I learn something new.

## Which brings me to what I hope was my point 

elisp is addictive
feedback loop
hacking is fun again

## If you've made it this far

Congratulations! Here's your reward:
<object width="480" height="385"><param name="movie" value="http://www.youtube-nocookie.com/v/mFnclBOlr6o&hl=en_US&fs=1&"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube-nocookie.com/v/mFnclBOlr6o&hl=en_US&fs=1&" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="480" height="385"></embed></object>


[eeclim]:
[eclipse]:
[m2eclipse]:
[aim]:
[eclim]:
