---
layout: post
title: "Working Title: Satisfaction"
published: true
categories: [inform]
---

I'm starting to settle into my new home office. There are boxes and
cables everywhere, but at least I have a desk to work from. Time to
get back to actual work again. 

For the past month, I have made virtually zero progress on my
Interactive Fiction game. I could blame this on the stress of moving
across the country while simultaneously negotiating building plans for
our summer house, and then having relatives visiting the new house
over easter, but I won't. Let's just say I need a little ignition to
get going again, and I'm hoping blogging about it a bit will help.

When I first concieved the idea for the game I thought I'd be done in
a day, or a weekend at most. That was back in January, and I think I
might be halfway to an alpha version now. Obviously, it took a whole
lot longer than I expected, mostly because I was learning
[Inform 7][i7] and designing the game world at the same time as
coding. One thing I did do right though, was actually writing a simple
design document before I started. It had a short synopsis describing
the game flow and central mechanics, and a section for each important
person in the game. Nailing down names, titles, and backgrounds of the
characters really helped things later on.

The games is divided into short chapters, with the viewpoint
alternating between two playable characters. I want the player to feel
like she's participating in a story rather than trying to beat a game,
so there are no obvious puzzles to solve. In fact, right now most of
the game can be played just by moving between rooms, and waiting at
the correct places. I'm considering using the subtitle *A Short Story
About Inevitability*, but we'll see.

Much of the plot revolves around discovering things about the player
characters, either by exploring or by having conversations with
NPCs. Even though the player is largely swept along for the ride, his
actions do have consequences for the ending of the game. There are
several different outcomes, even though they may not be obvious. 

There are two things I'm doing to invite re-play of the game. First,
I'm trying to pace it so that the player will never be able to get the
whole picture in the first play-through. The challenge here is making
the player feel intrigued rather than annoyed, and there's a cetain
risk this will backfire. Second, this will be a short game that can
probably be finished within the hour. 

As I mentioned earlier, I want this to feel like a proper story. This
means that the player should be able to behave like she would in real
life, and expect a reasonable outcome. It also means that I have to
deal with certain IF staples in creative ways. For example, realism
dictates that there should be certain props (like a sideboard with a
tea kettle and some breakfast on a sideboard in the Dining Romm) in
the game. I want the player to be able to pick up and interact with
these objects, but not carry them around the house (which would spoil
the overall tone of the story). Thus, I created a mechanism for
handling unimportant items:

<div class="i7-sample">
  <div class="i7-heading">Chapter - Non-essential props</div>
  <br/>
  <div>Things can be unimportant.</div>
  <br/>
  <div>A room has an object called the resting place. </div>
  <br/>
  <div>Carry out going when the player has an unimportant thing:</div>
  <div class="i7-indent-1">let the resting place be the resting place of the location;</div>
  <div class="i7-indent-1">let the inessentials be the list of unimportant things carried by the player;</div>
  <div class="i7-indent-1">repeat with the inessential running through the inessentials:</div>
  <div class="i7-indent-2">if the resting place is a supporter:</div>
  <div class="i7-indent-3">try silently putting the inessential on the resting place;</div>
  <div class="i7-indent-2">otherwise if the resting place is a container and the resting place is open:</div>
  <div class="i7-indent-3">try silently inserting the inessential into the resting place;</div>
  <div class="i7-indent-2">otherwise:</div>
  <div class="i7-indent-3">try silently dropping the inessential;</div>
  <div class="i7-indent-1">if the resting place is a supporter:</div>
  <div class="i7-indent-2">say <div class="i7-quote">"You put the <div class="i7-bracket">[the inessentials]</div> back on <div class="i7-bracket">[the resting place]</div>."</div>;</div>
  <div class="i7-indent-1">otherwise if the resting place is a container:</div>
  <div class="i7-indent-2">say <div class="i7-quote">"You put the <div class="i7-bracket">[the inessentials]</div> back in <div class="i7-bracket">[the resting place]</div>."</div>;</div>
  <div class="i7-indent-1">otherwise:</div>
  <div class="i7-indent-2">say <div class="i7-quote">"You leave the <div class="i7-bracket">[the inessentials]</div> in <div class="i7-bracket">[the printed name of the location in lower case]</div>."</div></div>
</div>

This rule ensures that an item that is declared `unimportant` the
player will drop it before leaving the room. To make it a bit more
realistic, you can name a container or supporter a `resting place`
(for want of a better name) for each room. Unimportant objects will
then be put back on the supporter or back into the container.

This is the (somewhat abridged) description of my Dining Room:

<div class="i7-sample">
  <div class="i7-heading">Section - Dining Room</div>
  <br/>
  <div>The sideboard is a fixed in place supporter in Dining Room. <div class="i7-quote">"Along one of the walls is a grand old sideboard crafted out of solid oak. Conveniently placed on it is <div class="i7-bracket">[a list of things on the sideboard]</div>."</div> The resting place of Dining Room is the sideboard.</div>
  <br/>
  <div>The silver kettle is on the sideboard. </div>
  <br/>
  <div>The kettle contains the tea. The description of the tea is <div class="i7-quote">"Smells like Earl Grey."</div> The indefinite article of the tea is <div class="i7-quote">"some"</div>.</div>
  <br/>
  <div>Some breakfast is on the sideboard. <div class="i7-quote">"It seems Watkins has prepared some cucumber sandwiches for you. Just as good, you sincerely doubt you'd be able to eat a full breakfast right now."</div> The breakfast is edible.</div>
  <br/>
  <div>The kettle and breakfast are unimportant.</div>

</div>

And this is how it plays:

    Dining Room
    Along one of the walls is a grand old sideboard crafted out of solid oak. 
    Conveniently placed on it is a silver kettle, some breakfast, some china and a 
    crystal decanter.

    >take kettle
    Taken.

    >take breakfast
    Taken.

    >i
    You are carrying:
      some breakfast
      a silver kettle
        some tea
      a candle (providing light)
      some clothes (being worn)

    >w
    You put the breakfast and silver kettle back on the sideboard.
    
    [New room description snipped]

Allright, I should stop now. I just realized I spent an hour on this
post instead of actually working on the darn game. See ya next time!

[i7]:http://inform7.com/


