# Zork v1 Group Project
Due: October 31 in class<br>
Medium: Printed paper submission

--------------------------------

Your team's mission is to design a text-based game engine capable of leading a user
through a thrilling adventure. It will use the individual Bork design as a foundation,
and expand upon it by allowing the adventurer to meaningfully interact with the game
state, solve puzzles, accumulate a score, potentially win the game, and heal/suffer and
possibly die.

- There are two halves to the mission: **standard features**, and **supplemental features**.

  - The **standard features** will be described in detail below. Your game engine
must work exactly as described, with .zork files in exactly the format given.
This is so that every team's engine works (in part) in the same way and I can
therefore reliably and efficiently test everybody's functionality.

  - The **supplemental features** are completely and totally up to you. I have
provided some ideas below. But I want your team to be as creative as you like,
inventing ways that the Bork experience can be entertaining, clever, and
surprising.

**Important:** no supplemental feature that you create may interfere with or
contradict a standard feature. Your program must be able to read the
standard <code>.zork</code> file structure so that your game engine can read
standard <code>.zork</code> files authored by myself and others. (You are free to, and
actually encouraged to, add features to your <code>.zork</code> files. However, the standard
file should still work. This means your program should work with <code>.zork</code> files
that include standard feature information and <code>.zork</code> files that include standard
feature information and supplemental feature information.)

# Standard Features

## Commands

Your Bork game engine must support the following two additional commands:<br>
<code>score</code> and <code>health</code>.

- **Score**
  - If the player types <code>score</code>, their numerical score, on a scale of your choosing, will be
displayed, along with a text "rank":
  - I place no constraints on how many points the player may be allowed to earn, or what
ranks you assign, other than when a dungeon is started from scratch, the player's score
must initially be zero. A player's score must be persisted and hydrated in the .sav file in some way (your
choice).

~~~~
> score You have accumulated 25 points. This gives you a rank of Amateur Adventurer. 
~~~~


- **Health**
  - The <code>health</code> command will display a "fuzzy" (i.e., non-numeric) message measuring
the shape of the player's physical condition:
  - I place no constraints on what kinds of messages you're capable of printing (be
creative, but appropriate please) or what events cause health increases/decreases, other
than when a dungeon is started from scratch, the player must initially be in good
health. A player's health must be persisted and hydrated in the <code>.sav</code> file in some way (your
choice).

~~~~
> health You feel fit as a fiddle.
...other commands...
> health You're a bit light-headed.
...other commands...
> health Each step is a stagger from the pain of your wounds.
...other commands...
> health You are about to die.
~~~~



## Events

An event is anything that can occur that changes the game state in some way. One
possible way of enabling such changes is to attach them to item-specific commands,
so that when an item is used in a certain way, an event is triggered.
Bork standard events will appear in the <code>.zork</code> file in [] syntax, as in the examples
below:

```
Trinkle Hall
Group Bork v1.0
===
Items:
Bomb 95
examine:The bomb is a heavy, glistening black sphere. On the top appears to be some
form of detonator.
kick[Wound(2)]:Ouch! That hurt your foot.
detonate[Die]:An ear-splitting halo of shrapnel kills you and seriously degrades the
room's interior.
---
DrPepper 10
kick:The can skitters down the hallway.
shake:A liquid fizzes menacingly inside the can.
drink[Transform(emptyCan),Wound(-1)]:Gulp, gulp -- that was GOOD!
---
emptyCan 2
kick:The empty can skitters down the hallway.
drink:Sorry, all gone!
stomp[Transform(squishedCan)]:The can crunches down into a thin disk, useful for
recycling.
---
squishedCan 2
throw:Zing!
stomp:Further stomping seems to have no effect.
recycle[Score(5),Disappear]:Boo-ya, helped save the environment.
---
magicWand 5
break[Wound(10),Disappear]:The wand snaps in half. Strange magic fills the air,
making you feel suddenly ill.
wave[Score(5),Teleport]:An angelic form briefly appears, smiles, and just as quickly
fades away.
---
StarWarsToy 5
touch[Score(1)]:Yoda says, "Do, or do not! There is no try."
break:Luckily, it's made of rugged plastic.
---
chainsaw 35
---
WawaTravelMug 10
refill[Win]:You refill the mug with everlasting light roast coffee, and live happily ever
after! :)
---
donut 7
eat[Disappear,Wound(-2)]:You feel mildly guilt-ridden.
stomp:The donut is now smooshed.
---
===

...the rooms section, exactly as in Bork...

```

As you can see, some item-specific commands are associated with one or more
events, which appear in a comma-separated list inside the brackets. This gives a crude
mechanism for interacting with the game state.

## Standard Bork events must include the following:

- **Score**
  - When triggered, adds the number of points to the user's score.

- **Wound**
  - When triggered, the user's health will decrease by the number of points in parentheses.
(If this is a negative number, this effectively doubles as a "Heal" event.) Reminder:
although the game internally keeps track of a player's health numerically, the "health"
command's messages do not print numbers, but fuzzy messages.

- **Die**
  - If triggered, the game is over, and the player loses.

- **Win**
  - If triggered, the game is over, and the player wins.

- **Disappear**
  - When triggered, the item will simply cease to exist, disappearing from the room,
inventory, and dungeon.

- **Transform**
  - When triggered, the item will disappear and be replaced by another item whose name
is in parentheses. (Normally the new item will not have existed previously; i.e.,
although it appears in the "Items:" section of the .zork file, it will not have been
named in the "Rooms:" section and therefore not really exist at all when the dungeon is
originally started.)

- **Teleport**
  - If triggered, the adventurer will be randomly teleported to some other room in the
dungeon. This may or may not be a room that they have already visited (or one that is
even reachable in any other way.)


## Supplemental Features

Below are some *example* features you may choose to add to your Bork engine, along
with an approximate number of points I think each one would be "worth" to design,
document, code, and test.

When you turn in your design, you should explicitly list each supplemental feature
you are choosing to add, along with your judgment of how many points it is worth.
(For features listed below, use that number. For new features you invent, compare
their complexity with the examples listed below and make a case for a particular
number of points).

### Examples: 
- **Verbose mode (+2p)**
  - Add commands "verbose on" and "verbose off" which will
toggle the exit text that currently always appears. (Having the exits listed at the end of
each room description makes it faster to go through the dungeon, but a real adventure
program would probably make the user infer what exits exist from the room
description.)

- **Hunger (+5p)**
  - The adventurer's health diminishes by (say) 1 point per turn, and is
only replenished by food, enough of which must be available somewhere and
somehow in the dungeon.

- **External clock (+5p)**
  - Some passage of time exists in a way the adventurer can
experience. A simple example of this is time of day: as the user plays the dungeon,
time will cycle from day to night and back again at an appropriate pace. Randomized
weather events would be another colorful choice. Fun fact: the real-life Zork III was
famous for an earthquake that took place after a certain number of moves, which
closed certain underground passages and opened others.

- **Light (+10p)** 
  - Some rooms, not all, will be completely dark to the adventurer unless
he/she has a light source (torch, spylaser, lightsaber, whatever) available and turned
on. Light sources must be items that are discoverable in the dungeon and which
"defuel" over time; i.e. there must be a finite length to how long they last before
burning out. It's your option as to whether light sources can be refillable. It's also your
option as to whether to include a grue (Google Zork grue to learn more J ).

- **Closed/locked exits (+15p)**
  - Some exits can be in a closed or locked state, and require
certain items ("keys") to be present and/or activated in order to open them.

- **NPCs (non-player characters) (+20p)** 
  - There are other creatures in the dungeon
besides just the adventurer. These creatures have their own position, state, inventory,
health, and possibly other attributes, and the player can only interact with them when
they encounter each other. Implementing some sort of dialogue -- asking, convincing,
threatening, trading items, etc. -- is even more of a bonus.

- **Combat (+25p)** 
  - A combat system enables NPCs to engage the adventurer (and each
other?) in battle. You're going to need weapons and rules of engagement to implement
this. Bonus points for assessing armament, shields, magic weapons and items, terrain,
strength and dexterity of characters, the influence of fighting in groups, and/or
anything else you might think of.

These are all only examples. There are lots and lots and lots of other things you could
do instead. Have fun with it!

##Submission

For this phase of your project, your team will be turning in the following deliverables:

1. **A list and description of each of your supplementary features.** This doesn't have
to be really long; just tell me briefly what each cool thing is designed to do in
your game engine. With each supplementary feature, include a point value
and argue briefly for why you think that point value is appropriate. In
many cases, it will be along the lines of, "we think our rotating room feature is
somewhat simpler, though not too much simpler, than the closed/locked exits
feature which was defined to be +15p, so we argue it is worth +12p."

2. **One or more UML class diagrams**, created and printed from a tool such as
[draw.io](https://draw.io "Draw.io") which we used in class, depicting the key classes, methods, attributes,
and relationships in your design. It is up to you exactly how much to show on
each class diagram(s), and it is also up to you how much of the original Bork
design to show on each diagram. You do not need to make a completely
exhaustive series of class diagrams that shows every single method of
everything, all the way back to Bork v1 in August. You do need to show
whatever is necessary to document and communicate the design decisions you
have made in this phase.

3. **Several UML sequence diagrams**, created and printed from a tool such as
draw.io, showing key interactions among objects and paths through the system.
A good rule of thumb is to include one sequence diagram for each cool feature
your system supports. For instance, you might show the sequence diagram
corresponding to the adventurer examining and eating a poison cupcake,
getting wounded as a result, and checking his/her health to see how bad that
really tasted. This would show your Wound and Disappear events as well as
your health command in action.

4. **Canonical examples of your <code>.zork</code> and <code>.sav</code> file formats.** These do not need to
be super long or clever, and you will not be graded on their creativity. Their
purpose is simply to document and define what the valid syntax for files will be
in your Bork program.

**To reiterate the above:** your Bork program must be able to read files in the
standard <code>.zork</code> file format described above. Any additional sections or
information you add must be done so in a way that will not conflict with what I
already have defined.

**To be clear:** your team will be turning in one submission for this group activity.

###Turning it in
To turn in this assignment, bring your **PRINTED** design packet to class. (Make sure
all team member's names are on it!)
