---
layout: post
title: Fun with Inform7
categories: [inform]
---

For the past month I've been amusing myself with a wonderful language:
[Inform7][i7]. It is a *very* domain-specific language in that its
main application is authoring text adventures (oh, sorry,
'[Interactive Fiction][if]'), and produces the [Z-code][zcode] or
[Gluglx][glulx] bytecode when compiled. 

There are several reasons I really shouldn't enjoy this language so
much:

 * It uses natural language, which I initially thought was a terrible
   choice for language design. I've always treasured brevity and
   conciseness in code, but I may have to reconsider. True, Inform7
   code is pretty verbose and sometimes a bit repetitive, but at the
   same time it flows like easy to read prose, kind of like Knuth's
   [literate programming][literate].
 * It comes with an IDE, which you are strongly encouraged to
   use. Normally I'm a die-hard emacs guy who only uses Eclipse as a
   last resort, but in this case it works pretty well. There is
   integrated documentation, a system for structured code, and testing
   facilities especially tailored for helping you run through
   different branches of you interactive stories.
   
Coding in Inform7 is an interesting experience. The code reads a lot
like regular english, but of course it isn't *really* natural
language. There is still a basic level of syntax you have to adhere
to, even if the parser does some fairly intelligent disambiguation of
terms and lets you describe you world in a lot of alterative ways. 

Yes, 'describe your world'. That pretty much sums it up; a program is
mostly a description of you model game world, with rooms, objects and
behaviours. This is a trivial treasure collecting game in five lines
of code (including the title):


``` inform7
    "Treasure Hunt" by Fredrik Appelberg

    Treasure Hill is a room. The description is "A green and abundant hill, 
             with a view of the surrounding fields. It makes an excellent 
             starting point for your treasure hunt."
  
    The chest is an closed openable container in Treasure Hill. It contains a 
        gold coin.
  
    The player is carrying a brass lantern. The player is wearing a cloak.
  
    Every turn when the player is carrying the coin: end the story saying "You 
          won!"  
```

Describing an object is the same as creating it. Here I create a room
(with a descriptive text), a chest with a gold coin inside, a lantern
and a cloak held by the player. The cloak is interesting: since I
state that the player is wearing it, Inform7 is able to infer that it
is a piece of clothing (an object subclass that has its own set of
properties) without me having to say it. Also, I've explicitly stated
that the chest is a container (another subclass), but in this example
that's not really necessary, since the compiler can infer that from
the fact that there's a coin inside the chest. However, I'll leave it
like this since it makes the text a bit easier to read.

For me, this is a very novel way of coding, and reminds me of the
[Prolog][prolog] hacking we did at the university: programming is
mostly just stating facts about the world, and trusting the compiler
to handle the rest.

Compiling and running is as simple as pushing a button. This is what a
transcript of the game looks like:


    Treasure Hunt
    An Interactive Fiction by Fredrik Appelberg
    Release 1 / Serial number 110211 / Inform 7 build 6G60 (I6/v6.32 lib 6/12N) SD
    
    Treasure Hill
    A green and abundant hill, with a view of the surrounding fields. It makes an excellent 
    starting point for your treasure hunt.
    
    You can see a chest (closed) here.
    
    >i
    You are carrying:
      a brass lantern
      a cloak (being worn)
    
    >x chest
    You see nothing special about the chest.
    
    >open it
    You open the chest, revealing a gold coin.
    
    >take the coin
    Taken.
    
        *** You won! ***
    
    In that game you scored 0 out of a possible 0, in 4 turns.

    Would you like to RESTART, RESTORE a saved game, QUIT or UNDO the last command?

But that's just my simple play-through. If you like, you can try the
game yourself, [without leaving your browser][play]. That's right:
Inform7 also generates a simple website for your game, complete with a
z-code interpreter written in javascript. 

![Creating Interactive Fiction With Inform 7](http://3.bp.blogspot.com/_j-AiRnNgy6s/TEt5OuqTwiI/AAAAAAAAASM/OA99j2FiNpw/s320/cifwi7cover-small.jpg)

If you don't hear from me for a while, it's probably because I'm busy
hacking my first serious game. It's taken me longer than anticipated,
but that doesn't bother me. Writing with Inform7 is *such joy* that I
can't help myself. 

By the way, I thought I'd take the time to plug Aaron Reed's excellent
[Creating Interactive Fiction With Inform 7][book]. It's one of the
most inspiring programming books I've ever read. And I've been through
a few.


[i7]:http://inform7.com
[if]:http://en.wikipedia.org/wiki/Interactive_fiction
[zcode]:http://en.wikipedia.org/wiki/Z-machine
[glulx]:http://www.eblong.com/zarf/glulx/
[literate]:http://en.wikipedia.org/wiki/Literate_programming
[prolog]:http://en.wikipedia.org/wiki/Prolog
[play]:/media/games/treasurehunt/play.html
[book]:http://www.amazon.com/Creating-Interactive-Fiction-Inform-7/dp/1435455061
