---
layout: post
title: JavaScript Race Conditions
date: 2015-02-25 15:08:00 +0200
categories: [js, coffeescript, node]
---

Yesterday I was rudely reminded that just because Node.js is single
threaded doesn't mean you don't have to worry about race
conditions. 

Our architecture is heavily message-oriented and the code that handles
ACKing messages looks something lite this. The idea is that
application code can call either `acknowledge()`, which acknowledges
that the task has been handled, or `retry()`, which means that the we
should retry (a copy of) the task at a later time and then acknowledge
that the original task has been dealt with. It should only be possible
to retry or acknowledge a task once, which is why we keep track of the
state in a variable `@resolved`.

{% highlight coffee %}
class Ack
    constructor: (@task, @queue) ->
        @resolved = false

    acknowledge: =>
        unless @resolved
            @task.acknowledge()
            @resolved = true

    retry: =>
        unless @resolved
            @queue.scheduleRetry(@task)
            .then =>
                @acknowledge()
{% endhighlight %}

Can you spot the error? It took me a while to realize that even though
`retry()` makes sure that `@resolved` hasn't been set and we call
`acknowledge()` to ACK the task and set `@resolved` immediately
afterwards, *that actually happens asynchronously*. We are using the
[Q][q] promise library here, which means that if `retry()` is called
repeatedly within the same process tick, then `scheduleRetry()` will
almost certainly be called multiple times. 

I think promises are wonderful, but the fact that they make
asynchronous code look synchronous can really trick you. I'm pretty
sure I wouldn't have made this mistake if I'd written this piece in
the regular old callback-oriented style.

[q]:https://github.com/kriskowal/q



