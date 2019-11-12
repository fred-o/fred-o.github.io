---
layout: post
title: "Weapons of Choice"
date: 2013-02-26 15:26
comments: true
categories: [emacs]
---

I do Java for a living. Apart from a two-year hiatus back in 2003-05
I've been a Java guy ever since I left university in 1997 (_Man,
sixteen years now! I didn't realize._).

Java is... well, it's Java. For all it warts, it's actually a pretty
decent environment to get things done in. Yeah, it's verbose as
hell. Yeah, most projects involve heaps of boilerplate XML. And yeah,
Maven is still the best we've got. But still, when you know your way
around it's not bad. The 3rd party library support is outstanding, and
most of it is Open Source. Performance is generally good. And the
cross-platform compatibility is at this point something we just take
for granted.

There have been other languages. I was heavily into Common Lisp for a
while. I would prefer it over java were it not for the lack of
standard libraries. [Clojure][clojure] seemed like a logical
progression, but for some reason I couldn't get into it. I love the
premise: "Functional programming with full access to the Java
ecosystem," but for some reason I never stuck around. 

I was on the [Scala][scala] wagon, I know enough [Ruby] to be
dangerous, and I've flirted with [Go], [Haskell], [Python], and
[CoffeeScript]. All excellent languages in their own right. None of
them really stuck. I've read the books, the blogs, the twitters. I
_know_ these are the languages the cool kids use nowadays. Maybe it
was me, maybe I wasn't hipster enough.

But there is one language that has grown on me. That hasn't gone away
or been replaced. It's a ugly little thing, full of warts and cruft
that has accumulated over many years. {"It is a Lisp, but it's awkward
and eccentric even by Lisp standards."} It lacks basic necessities like
namespaces and, for the longest time, lexical scoping. The standard
library is confused and naming conventions are all over the
place. Many Common Lisp staples (like ```loop```, ```map```, and
```reduce```) are missing. There is a Common Lisp compatibility
package, but using it was controversial and for a long time actively
discouraged.

I am, of course, talking about [emacs lisp][elisp]. I took stock of
[my github projects][github] and realized that apart from the Java
stuff, almost all of my recent activities are elisp. This was not
something I had planned, it's just that Emacs is such an awesome elisp
IDE. I'm more productive in elisp than in any other environment. When
I make code changes I'm not just connecting to a REPL, I'm actually
changing _the very environment I'm currently coding in._ This is
incredibly powerful. 

So, Java and elisp. Not very cool, I know. But do not underestimate
the power of knowing your tools and getting shit done.

[clojure]:http://clojure.org
[scala]:http://www.scala-lang.org/
[ruby]:http://ruby-lang.org
[go]:http://golang.org
[haskell]:http://www.haskell.org
[python]:http://python.org
[coffeescript]:http://coffeescript.org
[elisp]:http://en.wikipedia.org/wiki/Emacs_Lisp
[github]:https://github.com/fred-o
