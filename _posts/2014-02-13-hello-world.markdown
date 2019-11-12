---
layout: post
title: "Hello, World"
date: 2014-02-13 20:06
comments: true
categories: [node, js, pi]
---

{% img /media/images/helloworld.jpg 600 %}

Since I pretty much do [node.js][node] full-time nowadays, I was
curious if I'd be able to run it on my Pi as well. Turns out that's no
big deal; there are [binaries available][binaries] for download so you
don't have to go through the hassle of compiling (which I hear can
take quite some time).

Interfacing with the [PiFace][piface] is another matter. There is a
module called [piface-node][piface-node], but I couldn't get it to do
anything, and looking at the source code I could see that it was a bit
too low-level for what I wanted to do. However, I did find a C library
called [libpifacecad][libpifacecad], and started hacking to see if I
could write a node wrapper around it.

After a few evenings worth of work I am at a point where I can read
the sensors pretty reliably and writing to the screen somewhat less so
(it works maybe one time out of three). That's not the fault of the
underlying library though, it seems pretty solid. Rather, I suspect
there's either a race condition somewhere (possibly), or I'm making
assumptions about C++ memory management that's not entirely true
(probably), or I need to rethink how to do I/O in the node.js event
loop (almost certainly). 

[node]:http://nodejs.org
[binaries]:http://nodejs.org/dist/
[piface]:http://www.piface.org.uk/
[piface-node]:https://www.npmjs.org/package/piface-node
[libpifacecad]:https://github.com/piface/libpifacecad
