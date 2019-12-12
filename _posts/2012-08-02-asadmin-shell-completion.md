---
title: Asadmin shell completion
layout: post
published: true
---
I've been juggling [glassfish][gf] servers a lot lately, and found the
[asadmin] CLI admin tool pretty nice. There was one thing lacking
though: ZSH tab completion. So, being a hacker, I thought to myself:
"How hard can it be?"

Pretty hard, as it turns out. The ZSH completion system is
ridiculously complex and poorly documented. There is a
[pretty good ebook][guide], but it is from 2003 and I'm sure a lot of
things have changed since then. Other than that, then only tutorial on
writing your own completion functions was an
[old Linux Mag article][article].

After a lot of trial and error, borrowing code from various
[oh-my-zsh] plugins, and nearly giving up in frustration, I've now
produced a plugin of my own. It's pretty large (1147 lines), but has
completions for all the 265 subcommands that I know of. I quickly
realized that maintaining the argument completions for all these
subcommands would be quite impractical, so I did what any hacker would
do: I wrote [quick-and-dirty program][extractor] that goes through all
the glassfish jar files on the classpath, looking for classes that
implement the asadmin commands. It then looks at annotations and uses
a bit of introspection to determine what arguments are available for
each command, and what completions (if any) should be used to complete
these. The end result is a ZSH script fragment which I then insert
manually into my completion script. Overall, this has worked out
pretty well. And it sure beats doing it by hand.

Anyway, if you work with `asadmin` and would like some hot completion
action going, check out [my oh-my-zsh branch][code] on github.

[gf]:http://glassfish.java.net/
[asadmin]:http://weblogs.java.net/blog/felipegaucho/archive/2010/03/04/glassfish-v3-resources-administration-cli-tool-asadmin
[guide]:http://zsh.sourceforge.net/Guide/zshguide.pdf
[article]:http://www.linux-mag.com/id/1106/
[oh-my-zsh]:https://github.com/robbyrussell/oh-my-zsh
[extractor]:https://gist.github.com/3235533
[code]:https://github.com/fred-o/oh-my-zsh/tree/glassfish-plugin
