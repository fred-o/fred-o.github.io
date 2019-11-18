---
layout: post
title: Working with Java in Emacs (yet again)
categories: [java,emacs,eclim]
---

There are a couple of legacy Java services at work that I have to do maintenance
on every once in a while. I haven't really kept up with the state Java for the
past few years, so my approach goes something like "change as little as possible
and pray for minimal breakage". 

I used to rely on [eclim][eclim] back in the day, and am apparently still the
top contributor to the [emacs-eclim] project even though I threw in the towel
back in 2014. [Emacs-eclim][emacs-eclim] was kind of cool, but suffered from a
few serious drawbacks: 1) you still had to maintain an entire [Eclipse]
installation locally (which tended to rot away if left untended), and 2) for the
longest time there was only one guy working on the Emacs side of things.

![](https://raw.githubusercontent.com/emacs-lsp/lsp-mode/master/examples/logo.png)

There is now a far better solution though: use [LSP mode][lsp-mode] with the
[Java backend][lsp-java]. It's like emacs-eclim but better in every possible
way. There is a [well-defined protocol][lsp] for communicating between the
editor frontend and language server backend, which has been adopted across many
languages and editors. A community-driven project is a big deal.

As far as I can tell, there is still a headless [Eclipse][eclipse] server
running, but [LSP Mode][lsp-mode] handles installation and maintenance, so you
never need to bother with that stuff. UI response time is decent, and my emacs
behaves kind of like an IDE when I need it to. I like it; it makes my excursions
into Javaland at least bearable.

[emacs-eclim]: https://github.com/emacs-eclim/emacs-eclim

[eclim]: https://github.com/ervandew/eclim

[lsp-java]: https://github.com/emacs-lsp/lsp-java

[lsp-mode]: https://github.com/emacs-lsp/lsp-mode

[lsp]: https://microsoft.github.io/language-server-protocol/

[eclipse]: https://www.eclipse.org/eclipseide/
