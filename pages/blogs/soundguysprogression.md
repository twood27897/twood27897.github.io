---
layout: post
title: 'Progression In Sound Guys'
author: Thomas Wood
tagline: 'Progression In Sound Guys'
description: Progression In Sound Guys
---

<p align="center">
  In <a href="https://twood27897.github.io/pages/sound-guys.html">Sound Guys</a>, I knew from the outset I wanted the game to be structured with the "watch big number go up" mantra of an idle/incremental game - I wanted the player to start with very few of these devious sound guys I'd created and (with a satisfyingly quick pace) gain more of them over time. Two challenges presented themselves pretty quickly however. One, in balancing the cost of buying new guys vs how much they gave back so that the pace was satisfyingly quick and players chased the carrot on the stick of collecting and placing new sound guys. And then two, in keeping the player created song(s) sounding good-ish once they were chasing the carrot on the stick and were incentivised to keep acquiring and adding more and more guys. In this blog I want to cover how I tackled these problems and reflect on what went well (and what, well, didn't!).<br/><br/>

<b><i>1. Balancing Cost vs Earnings of Guys</b></i><br/><br/>
  So, to start, how much should sound guys cost/earn? "Easy!", you might say, "Small numbers to start and then bigger numbers later on", and, to be honest, that isn't far off. But the devils in the details! I found, when just guess-timating and semi-randomly assigning costs and earnings that I ended up with inconsistent pacing; periods where players flew through buying new guys; but, more importantly, gaps where players weren't able to afford any new guys for too long and got bored. Bored! Uh oh!<br/><br/>

  [image of sound guys packs with ??? cost]

  What next? First, from watching people play I could see where the pacing problems occurred. I could adjust the cost or earnings of any guys that slowed the pace too much - bring the costs down or push the earnings up to shorten the time till the next new guys. This worked! But also meant I wasn't far off from flying blind. Whenever I added new guys I would guess-timate then have to play myself or even watch someone else play to know if the cost/earning for the new guys didn't create too much downtime. I knew I needed a different approach. Luckily, this didn't take long. I was still early in development when I took a step back and changed my approach. "Changed my approach to what?", you ask? "MATH!", I shout right back.

  Anyone who has been around games with number driven progression for awhile can guess that there is some kind of funky sum going on behind the scenes. Whether it's XP requirements to hit the next level in an RPG or the cost of the next upgrade in an idle game, the numbers look too random yet specific to have been guess-timated and tweaked by a developer. I knew this, but what kind of funky sum was I going to pick? Some kind that can be made in a graph, probably.

  I knew that I wanted players to gain a solid base of guys pretty quickly but that I was happy for new guys to be acquired less often as time went on - this felt right to me as later in the game players would already have lots of guys to play and experiment with and I wanted to make sure there was room to do that. Knowing this I could work backwards and realise that not only should the cost/earnings of guys start small and end big, but also, the difference between the cost and earnings of guys should get larger as the game went on to allow for more downtime (meaning time to play and experiment) before new guys were acquired. If that was a graph it would be this one:

  [linear graph with cost and earnings over time]

  None of this is groundbreaking, most progression systems hit the gas and reward the player more regularly at the beginning of a game before slowly taking their foot off the gas once the player is settled in, has more to play with and is enjoying themselves while hopefully not feeling the mounting pressures of their own sunk cost too deeply. But don't take my word for it, look at Clash of Clans, an old master of progression, and the difference in time for upgrading it's townhall to level 2 vs level 17:

  [level 1 (10s) and level 17 (12d) townhall with time to build underneath]

  If they can get away with making their players wait 103,680 times longer (!) for their last upgrade vs their first, we can probably all push the boat out a bit further.   
  
  I mean, we can always assume the two numbers are dependent on each other. As a player, we would assume a game element that is valuable costs a lot and vice versa. For Sound Guys, this means a guy that earns a lot should cost a lot and a guy that costs a lot should earn a lot. Cost and earning are proprotional. At this point, math equations can begin to rear their ugly head.<br/><br/>

<b><i>2. Keeping Everything Sounding Good</b></i><br/><br/>
  Now that players enjoyed the process of building their collection of guys, a new problem presented itself. How do you keep a piece of music sounding good when the person making it is being incentivised by a nefarious game developer to keep adding more and more notes to it till it sounds as good as a piano falling down a flight of stairs (which is to say not good at all)? Well, your first thought might be to lock the game dev up and throw away the key - but what if the game dev is you? And what if you don't like being locked up? That was the problem I faced. So what did I do? Designed some solutions! And then picked some solutions!<br/><br/>
</p>
