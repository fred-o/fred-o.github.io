---
layout: post
title: "Something Cool (Pt. 2)"
date: 2013-06-17 21:29
comments: true
categories: [js, coffeescript]
---

In an [earlier post][earlier] I wrote about a way to implement `cons`,
`car` and `cdr` (also known as `pair`, `first`, and `second`) using
only function definitions and if statements in javascript. That was
pretty mindblowing for me, but it turns out there is an a way to do
_without even using branching logic_.

``` javascript pair
function pair(x, y) {
    return function(s) {
	    return s(x, y);
	};
};
```

The pair function take to arguments X and Y, and returns another
function which takes a function argument S. It is essentially a
promise to apply S to X and Y.

``` javascript first and second
function first(p) {
    return p(function(x, y) { return x; });
}

function second(p) {
    return p(function(x, y) { return y; });
}
```

Implementing `first` and `second` becomes pretty straightforward; it's
just a matter of taking the output from a call to `pair` and applying
it to a function taking two arguments X and Y and returns either X
(`first`) or Y (`second`).

The CoffeeScript implementation is incredibly terse and elegant,
reading almost like mathematical notation.

``` coffeescript The CoffeeScript version
pair = (x, y) -> (s) -> s x, y
first = (p) -> p (x, y) -> x
second = (p) -> p (x, y) -> y
```

[earlier]:{% post_url 2010-04-26-something-cool %}
