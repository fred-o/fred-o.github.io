---
layout: post
title:  "Moving to Linux: some notes"
date:   2020-09-09 14:41:50 +0200
categories: [linux]
---

I thought I landed on [Regolith][regolith] for daily use, but the
[Cinnamon Desktop][cinnamon] keeps dragging me back. It's just so
darn neat. I like how sensible most of the defaults are, how snappy
the Software Manager is and that I don't have to jump through a lot of
hoops to be able to install FlatPaks. Linux Mint feels like a _proper_
OS; something that's just not patched toghether from a disparate code
bases from across the internet.

That said, there were a couple of minor things that bugged me: 

 * Neither Mint or Regolith were able to find my Pixma MG6150 printer
   by default, although XSane had no problems scanning documents. The
   fix was to install the [cups-backend-bjnp][bjnp] package, and then
   everything just worked.
 * Emacs didn't display color emojis correctly until i recompiled with
   `--with-cairo` and added this line to my `init.el`:
```elisp
(set-fontset-font t 'symbol "Noto Color Emoji" nil 'append)
```
 * Bluetooth is kinda meh. I can pair with my wireless headphones
   pretty easily, but can't use it for input and output at the same
   time. Re-connecting the headphones after I've used them with my
   mobile is a hassle. As a result I tend use my phone for video
   conferencing.
 * There is something off with the touchpad. I can't quite put my
   finger on it (heh), but the one I had on the MacBook was very
   precise, and this one is not. Maybe it's a calibration thing; I'll
   see if I can work it out.


[bjnp]:https://packages.debian.org/sid/cups-backend-bjnp
[regolith]:https://regolith-linux.org/
[mint]:https://linuxmint.com/
[cinnamon]:https://en.wikipedia.org/wiki/Cinnamon_(desktop_environment)
