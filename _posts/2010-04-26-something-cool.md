---
layout: post
title: Something Cool
categories: js
---

I found something cool on my harddrive today; an old JavaScript
snippet that I wrote some years ago when I was heavily into Lisp. Yes,
Lisp. I love Lisp. It's the mother of all languages and in a perfect
world I'd spend my days hacking cool stuff in Lisp or Clojure rather
than maintaining a behemoth of a Java web app that I secretly hate.

Anyway, even if you've never heard of Lisp you need to know two things:

 1. In Lisp, you work very close to your data structures. Everything,
including the code that you write, is expressed as lists or variants
of lists.
 2. The most basic data structure is what is known as a **cons
 cell**. Simply put, a cons is **CONS**tructed from two arbitrary values,
 which can be either primitive or a pointers to other cons cells. 
 
Lisp has a function called `cons` which takes two arguments and
returns a cons cell. Two other functions, named `car` and `cdr` for
historic reasons, takes a cons cell as argument and returns the first
or second value respectively. This doesn't seem especially exciting
but is cool, since if you have these three functions you can create
linked lists, and all other structures (trees, hash tables, etc.) can
be expressed in terms of lists.

So, in a minimal environment where you don't even have basic data
structures such as arrays, structs or objects with value slots, how
exactly do create cons cells? Well, it turns out that as long as you
have a facility for defining and applying functions, and a branching
mechanism, you can express cons cells in a functional manner:
 
``` js
function cons(a, b) {
    return function(d) {
	return d == 0 ? a : b;
    };
}

function car(c) {
    return c.call(null, 0);
}

function cdr(c) {
    return c.call(null, 1);
}
```

This idea isn't mine; it has been around for ages in Lisp and Scheme,
but I implemented it in JavaScript to see what it would be like. I
find this construct very simple and beautiful. By calling the `cons`
function we create a closure with the variables `a` and `b` bound to
the supplied arguments. `cons` then returns an anonymous function
which is simply a promise to return either `a` or `b`. 

``` js
> var c = cons(17, 23);
undefined
> car(c)
17
> cdr(c)
23
```

Of course, we could to without the `car` and `cdr` functions; they are
merely syntactic sugar. Since the value of `c` is a function we can just
call it directly:

``` js
> c(0)
17
> c(1)
23
```

Finally, this is how we'd go about creating and traversing that linked
list:

``` js
> var l = cons(1, cons(2, cons(3, null)))
undefined
> for(i = l; i != null; i = cdr(i)) { console.log(car(i)); }
1
2
3
undefined
```

So, using only functions and branching logic we can derive just about
*any* data structure. I remember learning about this in a CS class
where we used a lot of Scheme, and it was mindblowing. 

It still is.
