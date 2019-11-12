---
layout: post
title: "Node-pokeapi"
date: 2015-03-31T08:46:34+02:00
---

I built a simple [node.js] client for fetching Pokémon data from
[pokeapi.co]. You can run it on the server as well as in a browser,
although in the latter case you'd need to direct the requests through
a proxy on your origin server.

{% highlight coffee %}
PokeApi = require 'pokeapi'
api = PokeApi.v1()

api.get('pokemon', 1).then (bulbasaur) ->
    api.get(bulbasaur.moves).then (moves) ->
        console.log 'here the move list for', bulbasaur.name
        console.log move.name for move in moves
.fail (err) ->
    console.log err
{% endhighlight %}

Get the [npm here][npm], or check out the [code here][gh].

[node.js]:http://nodejs.org
[pokeapi.co]:http://pokeapi.co
[npm]:https://www.npmjs.com/package/pokeapi
[gh]:https://github.com/fred-o/pokeapi
