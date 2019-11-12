---
layout: post
title: "Hardware is hard"
date: 2014-03-15 13:41:55 +0100
comments: true
categories: [pi]

---

Where I live there aren't really any options for electronics shopping,
so I was resigned to do it mostly through mail order. Fortunately I
discovered that [Kjell & Co.][kjell], which is probably the closest
thing to Radio Shack we have in Sweden, not only carries a lot of
Arduino stuff, but also has an shop in the next town over (Luleå,
about 50km away).

So I got myself a soldering station, some lead-free solder[^1] some
sensors and grab bag of diodes, resistors and cables to play with. It
must be something like 25 years since I last had a soldering iron in
my hands, but putting the [T-cobbler][cobbler] together I all just
came back to me. Even the smell was exactly as I remembered it.

{% img /media/images/wired.jpg 600 %}

I had some problems getting the [DHT11][dht11] sensor up and
running. All the examples I could find had a four-legged sensor, but
mine came pre-mounted on a board with only three. Also, the
configuration of the legs had been switched around. That one took some
figuring out.

The [DS18B20][ds18b20] however, just plain refused to show up as a
device in `/sys/bus/w1/devices`. I tried everything: reflashed the SD
card, checked the kernel messages, double-checked my connections
(twice), moved things around on the bread board in case there was a
break (which incidently there is; the vertical strips break in the
middle, but this didn't affect my wiring) and tried every combination
of power and GND I could find on the breakout board in case I had done
a bad soldering job (which I hadn't). I did countless reboots. Still
nothing.

I had just about written that sensor off when I discovered that the
pullup resistor[^2] I'd used wasn't yellow-purple-red as specified, but
yellow-purple-yellow. 470K instead of 4.7K. Oops.

Hardware is hard. 

[^1]: Which everyone seems to think is terrible, but I haven't had any problems with. Yet.
[^2]: I confess I have no idea what a pullup resistor actually does.

[kjell]:http://kjell.com/
[cobbler]:http://www.adafruit.com/products/1105
[dht11]:http://www.adafruit.com/products/386
[ds18b20]:http://www.adafruit.com/products/381
