---
layout: post
title: "Emacs Protip: other-window commands"
date: 2013-07-11 15:33
comments: true
categories: [emacs, protip]
---

In order to get better at emacs I'm focusing on a few keyboard
shortcuts at a time in order to *really* grok it and start using them
effectively.

I'm starting out with the `other-window` family of commands. There are
quite a few of them, but these are some of the more useful. They are
pretty easy to memorize in that they are variations of other common
commands and share the same keyboard shortcut except for that infix
`4`.

### dired-other-window (C-x 4 d)

Runs dired in the other window. I don't use dired that much, but I can
see how this comes in handy.

### find-file-other-window (C-x 4 f)

Open a new file in the other window. This is really useful, as I often
open a new file, split the window in two, go back to the original
window and switch back to the original file. This command does all
that at once.

### ido-switch-buffer-other-window (C-x 4 b)

Switch to another buffer in the other window. Again, this is something
that I do a lot, and I'm happy there's a shortcut for it.

If you have [ido] installed (and you should), the latter two will bind
to `ido-find-file-other-window` and `ido-switch-buffer-other-window`
respectively. They do more or less the same thing, _only better._

[ido]:http://www.emacswiki.org/emacs/InteractivelyDoThings

