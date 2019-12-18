---
layout: post
title:  Ditching GMail
date:   2019-12-13 12:19:23 +0100
---

I remember when GMail was cool. If you managed to snag an invite you got a
whooping 1GB of storage, which was amazing for its time, and it pretty much
solved the spam problem once and for all. Google was a different company back
then, and could seemingly do no wrong.

15 years later, I'm ditching GMail. We simply cannot trust Google to not be evil
anymore. 

[Mailu] is a collection of Docker images that together form a complete mail
server setup, with spam filtering, IMAP and webmail. It's pretty easy to set up;
you fill in a form with your requirements, they generate a `docker-compose.yml`
which you put on your (docker-enabled) server, and that's pretty much it. I was
up and running on the smallest DigitalOcean droplet available in an hour, and
after some DNS record juggling (the Mailu [documentation] is really helpful) I'm
now pretty much self-reliant.

10/10 would recommend.

[Mailu]:http://mailu.io
[documentation]:https://mailu.io/1.7/dns.html
