---
layout: post
title: The hidden costs of Unit Tests
published: false
---

Unit tests are hard. Oh, not to write, mind you. That's the easy
part. Maintaining them however, is harder.

How do you know that your tests actually test what they're supposed
to? Do you write 

At my current project we have more than 2000 painstakingly crafted
test cases accumulated over five years or so. We actually hit 3000 at
one point, but that number dropped drastically when we retired our old
Struts-based frontend.



Ironically, maintaing the unit tests typically requires more effort
than regular code since, well, there are no unit tests for them.

* Poorly documented
* Usually a lot of copy-and-paste
* Can't use refactoring tools

