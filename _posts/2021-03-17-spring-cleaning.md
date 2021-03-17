---
layout: post
title:  Spring Cleaning Your .emacs.d
date:   2021-03-17 10:15:15 +0100
category: [emacs]
---

A good practice is to ditch your existing `.emacs.d/init.el` every now
and then. Start over with a fresh file and add back the stuff you
really need, when you need it. 

 * Make sure you know what every line you put in `init.el` actually
   does.
 * Reassess your keybindings. Which do you need for everyday use? I
   had a pretty elaborate Spacemacs-inspired setup where every command
   I could think of was tied to a chord under the `C-c` prefix. But
   that was overkill. I start `notmuch` maybe once a day, and
   `easy-jekyll` once a month. Maybe they don't need dedicated
   keybingings? On the other hand, I've found that I want to access
   recent files almost constantly, so it makes sense to bind it to
   something like `C-c f r`.
 * In the same vein, think about what modules you are actually
   using. I kept including `yasnippet-snippets` for the longest time
   because I was using `yasnippet` anyway and wouldn't it be nice to
   have this huge library of snippets created by strangers on the
   internet? Turns out those strangers always had different opinions
   of coding style than me and I wasn't using their snippets at all.
 * This is a good opportunity to look for new things to incorporate in
   your workflow. The Emacs ecosystem is pretty vibrant, and new
   interesting ideas keep popping up. This is what I'm playing around
   with right now:
   
   + Inspired by [Protesilaos
	 Stavrou](https://www.youtube.com/channel/UC0uTPqBCFIpZxlz_Lv1tk_g),
	 I'm testing out `selectrum` and `consult` instead of `ivy` and
	 `consul`, and I like it so far. The idea is to use independent
	 modules that work together by hooking into already existing Emacs
	 APIs instead of building new abstractions on top of the stack.
   + I was a long-time `company-mode` user, but I've dropped it in
     favor of Emacs's native `completion-at-point` system, at least
     for the time being.
   + `projectile` is great and all, but I recently discovered that
     Emacs actually has its own built-in project management system,
     called `project.el`, which covers my (very limited) use case. I
     mostly need to open a file or switch to a buffer in the same
     project, or close all open buffers belonging to a
     project. So it turns out I don't actually need `projectile`?

<iframe width="560" height="315" src="https://www.youtube.com/embed/43Dg5zYPHTU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
