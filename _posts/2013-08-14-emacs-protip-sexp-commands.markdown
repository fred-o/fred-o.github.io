---
layout: post
title: "Emacs Protip: sexp commands"
date: 2013-08-14 08:58
comments: true
categories: [emacs, protip]
---

After having internalized the [other-window commands][other], I'm now
focusing on commands working with balanced expressions (or
[s-expressions][sexp]). Depending on the current buffer mode, this can
be a word, a string literal, an XML tag or a LISP expression. The ones
I've found most useful are:

### forward-sexp (C-M-f)
### backward-sexp (C-M-b)

Move forward or backward over a balanced expression. Feels way more
intuitive than `forward-word`/`backward-word` when navigating code.

### kill-sexp (C-M-k)

Kill the balanced expression in front of the cursor (i.e. the one you
would jump over with `forward-sexp`. I find this a lot more efficient
than repeating `kill-word` over and over.

### transpose-sexps (C-M-t)

I admit I haven't used this a lot. It's like `transpose-chars`, but
works on balanced expressions.

[other]:{% post_url 2013-07-11-emacs-protip-other-window-commands %}
[sexp]:http://en.wikipedia.org/wiki/S-expression
