---
layout: post
title: "Raspberry Pi"
date: 2013-02-10 15:38
comments: true
categories: [pi, jfokus]
---

The main focus at the [JFokus][jfokus] conference last week seemed to
be the [Raspberry Pi][pi]. Sure, Oracle was pushing Java 8, JEE 7 and
whatnot, but everybody was talking about the Little PC That
Could. There were raffles where you could win them, and according to
rumors they were given away to the audience at the embedded systems
keynote.

I happened to attend a technical session, mostly because there was
nothing better on. [Gerrit Grunwald][gerrit] showed off a Pi-based
system he had set up to display the temperature in his home, and I was
sold immediatly. As I left the room I flipped open my laptop, found [a
reseller][mc] and ordered myself a starter kit. A few hours later I
got a notice saying it had shipped and by early afternoon the next day
it had been delivered, a full two days before I got home myself.

These are my impressions after playing with it over the weekend:
 
 * It really is small. Really, really small. Of course, I seem to have
   a thing for [small computers][nano].
 * Video output is strictly HDMI. It hooked up to the TV and booted
   nicely, but when I tried connecting it to my old 17" monitor using
   a HDMI-DVI converter I couldn't get any signal at all. It doesn't
   really matter all that much, as getting a SSH server up and running
   was trivial, and I kind of prefer accessing it through the network
   anyway.
 * I had the notion that [Go][go] would be a good language to do some
   systems programming in, but getting the infrastructure in place was
   not trivial. Essentially you have to
   [apply a small patch and rebuild the debian packages][installing]
   before installing, and remember to put `export GOARM=5` in your
   `.bashrc`.
 * The CPU isn't great. Building Go from scratch took me well over 30
   minutes. I realize I may have been spoiled by my MacBook.

And now I have a trivial webserver running:

``` go Disclaimer: I don't really know Go
package main

import (
	"fmt"
	"net/http"
	"html"
	"log"
	)

var port = 8001

var s = &http.Server {
	Addr: fmt.Sprintf(":%d", port),
}

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hullo, %q", html.EscapeString(r.URL.Path))
	})
	log.Printf("starting web server on port %d", port)
	log.Fatal(s.ListenAndServe())
}
```

[jfokus]:http://jfokus.se
[pi]:http://www.raspberrypi.org
[mc]:https://www.microkit.se/
[gerrit]:https://twitter.com/hansolo_
[nano]:{% post_url 2010-05-11-the-anti-ipad %}
[go]:http://golang.org
[installing]:http://www.greenhughes.com/content/how-get-go-going-raspberry-pi

