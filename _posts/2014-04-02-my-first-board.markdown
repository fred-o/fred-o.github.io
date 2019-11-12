---
layout: post
title: "My first board"
date: 2014-04-02 08:58:08 +0200
comments: true
categories: [js, node, pi]
---

I'm immensely proud of the fact that I soldered together my first
prototype board last night. It holds two [DS18B20][ds18b20]s; one
naked sensor soldered straight onto the board and one on a waterproof
cable that I've hung out the window. This lets me measure current
indoor and outdoor temperature.

![](/media/images/prototypeboard.jpg)

The board is connected to my Raspberry Pi, where a [node.js][node]
program using [sensorjs][sensorjs] is regularly sending the sensor
data to a [TempoDB][tempodb] database.

[ds18b20]:http://learn.adafruit.com/adafruits-raspberry-pi-lesson-11-ds18b20-temperature-sensing/ds18b20
[node]:http://nodejs.org
[sensorjs]:https://www.npmjs.org/package/sensorjs
[tempodb]:https://tempo-db.com/
