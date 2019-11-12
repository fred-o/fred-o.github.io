---
layout: post
title: inform-htmlizer.el
published: true
categories: [emacs, inform]
---

While writing the previous two posts I found the need to display
[Inform7][i7] source code in a blog-friendly way. Unfortunatly, the
[pygments][pyg] source highlighter doesn't support it, and I couldn't
find any plugins for it that would. The closest I came was this
[blog post by Max Battcher][snippet], but as he mentions, pygments is
probably a bad fit for this kind of code. Inform7 source is best
displayed with a non-fixed-width font and uses regular line wrapping,
but depends on significant whitespace for control flow. As far as I
could tell, pygments doesn't really support that kind of output, and
anyway I couldn't figure out how to use Python's `setuptools` to hook
up my own plugins to begin with.

So I did what any self-respecting hacker would do: I rolled my own
solution using the tools I'm used to. In my case that means
[elisp][el], which is a good fit for med since Emacs is the tool
[I'm using][setup] to write this blog. I've put the code on
[GitHub][github] it case it proves useful for the very small subset of
Inform7 users who are also a) Emacs fanatics, and b) desperately in
need of a HTML syntax highlighter.

[i7]:http://inform7.com
[pyg]:http://pygments.org/
[snippet]:http://blog.worldmaker.net/2009/feb/17/code-snippet-moment-inform-7-lexer-pygments/
[el]:http://en.wikipedia.org/wiki/Emacs_Lisp
[setup]:{% post_url 2010-04-23-my-setup %}
[github]:https://github.com/fred-o/inform-htmlizer
