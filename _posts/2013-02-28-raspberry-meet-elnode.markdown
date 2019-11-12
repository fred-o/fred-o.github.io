---
layout: post
title: "Raspberry, meet elnode"
date: 2013-02-28 19:08
comments: true
categories: [emacs, pi]
---

So, I was curious about [elnode][en]. And I wanted to play more with
my [Raspberry Pi][pi]. It only seemed logical
[to combine the two.][example] 

You know, because I can.

_(I promised [@nicferrier][nic] a writeup on this project, so here
goes.)_

[pi]:http://www.raspberrypi.org
[example]:http://pi.appelberg.me:8000
[nic]:https://twitter.com/nicferrier

## Installing Emacs 24

The Emacs that comes with the Raspian distribution is version
23.4. Unfortunately, [elnode][en] requires Emacs 24, so I had to
compile my own. I dreaded doing this, but it actually turned out
pretty straightforward. 

You need to `apt-get install` some build dependencies like
`libncurses-dev` (and probably `build-essentials` if you don't
already have them), then you're ready to go:

```
 > wget http://ftp.gnu.org/pub/gnu/emacs/emacs-24.2.tar.gz
 > tar xvfz emacs-24.2.tar.gz
 > cd emacs-24.2
 > ./configure
 > make 
 > sudo make install
```

This would be a good opportunity to make a sandwich, or
[learn some sanskrit][sanskrit], as building everything will take
quite a while.

[sanskrit]:http://www.penny-arcade.com/comic/2008/02/06

## Running Emacs

By default the elnode webserver runs on Emacs startup, so the idea is
to fire up emacs and just leave it. One way of doing that would be to
ssh into the Pi and run emacs inside a [tmux] session (I tend to use
tmux for all remote access anyway). This makes the emacs process
persistent, and when I next log in I can just resume the session.

However, with this approach I'd still need to login in and start
manually every time the server restarts, so the setup is kind of
fragile. Especially since I have builders running round the house
cutting the main power every now and then.

So instead I use a simple init script to run emacs in daemon mode
(using the aptly named `--daemon` switch). This way I can still
connect to the running emacs process using `emacsclient`, but I don't
have to bother with starting it manually anymore.

The only caveat is that since I want `start-stop-daemon` to fork off a
new process to run emacs as the `pi` user (running a web-connected
emacs as `root` seems like a really bad idea), I can't rely on it to
generate a correct PID file for me. Instead, we'll have to do that
from inside emacs (see next section).

[tmux]:http://tmux.sourceforge.net

### /etc/init.d/elnode
``` bash
#!/bin/sh

set -e

# Must be a valid filename
NAME=elnode
PIDFILE=/home/pi/elnode/elnode.pid
UID=1000
#This is the command to be run, give the full pathname
DAEMON=/usr/local/bin/emacs
DAEMON_OPTS="--daemon --load /home/pi/.emacs.d/init.el --load /home/pi/elnode/web.el"

export PATH="${PATH:+$PATH:}/usr/sbin:/sbin"

case "$1" in
    start)
        echo -n "Starting daemon: "$NAME
        start-stop-daemon --start --quiet --pidfile $PIDFILE --chuid $UID --exec $DAEMON -- $DAEMON_OPTS
        echo "."
	    ;;
    stop)
        echo -n "Stopping daemon: "$NAME
        start-stop-daemon --stop --quiet --oknodo --pidfile $PIDFILE
        echo "."
	    ;;
    restart)
        echo -n "Restarting daemon: "$NAME
        start-stop-daemon --stop --quiet --oknodo --retry 30 --pidfile $PIDFILE
        start-stop-daemon --start --quiet --pidfile $PIDFILE --chuid $UID --exec $DAEMON -- $DAEMON_OPTS
	    echo "."
	    ;;

    *)
	    echo "Usage: "$1" {start|stop|restart}"
	    exit 1
esac

exit 0
```

When I start the deamon I get some weird error messages from elnode,
but as far as I can tell it actually runs fine.

```sh
pi@raspberrypi ~ $ sudo /etc/init.d/elnode start                                                                                                                  
Restarting daemon: elnode("/usr/local/bin/emacs" "--load" "/home/pi/.emacs.d/init.el" "--load" "/home/pi/elnode/web.el")
Starting Emacs daemon.
deleting server process
elnode-error: elnode--sentinel 'deleted.' for process  *elnode-webserver-proc* with buffer *elnode-webserver*
found the server process - NOT deleting
elnode-error: Elnode server stopped
.
```

## Running elnode

Elnode is available from [Marmalade][marm], which makes it very easy
to install from inside emacs. I also grapped the [xmlgen] library to
help with HTML generation.

This is my little Hello World-script. There's nothing remarkable about
it, really. The first section generates a PID file to be used by the
init script to check if the process is running. I stop the default
server, as it binds to "localhost", and I want to bind to the actual
server name (`raspberrypi.local` in my case) in order to serve remote
requests. You can see it in action [here.][example]

Did I say there was nothing remarkable about this script? _Yeah, apart
from the fact that it's running a webserver inside a 25 year old text
editor on a computer that's about as large as a deck of cards._

[marm]:http://marmalade-repo.org/
[xmlgen]:https://github.com/philjackson/xmlgen

### /home/pi/elnode/web.el
``` elisp
;;; -*- lexical-binding: t -*-

;; write the pid file
(with-temp-file "/home/pi/elnode/elnode.pid" (insert (format "%d" (emacs-pid))))

(require 'cl)

;; stop the default server
(elnode-stop 8000)

(defvar request-count 0)

(defun hello-handler (httpconn)
  "Hello, world" 
  (elnode-http-start httpconn 200 '("Content-Type" . "text/html"))
  (elnode-http-return httpconn
		      (xmlgen 
		       `(html
			 (head
			  (title "Hello, elnode!")
			  (style "body { margin:100px auto auto auto; width: 400px; }"))
			 (body
			  (p "Hello, I am a small "
			     (a :href "https://github.com/nicferrier/elnode" "webserver")
			     " running inside Emacs 24 on a "
			     (a :href "http://www.raspberrypi.org" "Raspberry Pi")
			     ".")
			  (p "The local time is "
			     ,(format-time-string "%H:%M")
			     " and I have served a total of "
			     ,(incf request-count)
			     " requests."))))))

(defun root-handler (httpconn)
  (elnode-hostpath-dispatcher httpconn '((".*/favicon.ico" . elnode-send-404)
					 (".*" . hello-handler))))

(elnode-start 'root-handler :port 8000 :host "raspberrypi.local")
```



[en]:http://www.raspberrypi.org
