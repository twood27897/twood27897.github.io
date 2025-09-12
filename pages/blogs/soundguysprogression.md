---
layout: post
title: 'Progression In Sound Guys'
author: Thomas Wood
tagline: 'Progression In Sound Guys'
description: Progression In Sound Guys
---

<p align="left">
  In <a href="https://twood27897.github.io/pages/sound-guys.html">Sound Guys</a>, I always wanted the game to be structured with the "watch big number go up exponentially" mantra of an idle/incremental game - for the player to start with very little and (with a satisfyingly quick pace) gain more and more and more over time. Two big challenges presented themselves pretty quickly, however. The first I will talk about in this blog - balancing the cost of buying new guys vs how much they gave back so that players chased the carrot on the stick of collecting and placing new sound guys. The second, in keeping the player created song(s) sounding good-ish once they were chasing the carrot on the stick and were incentivised to keep acquiring and adding more and more guys, I will write about at another time.<br/><br/>

<b><i>Balancing Cost vs Earnings of Guys</i></b><br/><br/>
  So, how much should sound guys cost/earn? And how should that change as the game progresses and the big number gets exponentially bigger? "Easy!", you might say, "Small little numbers at the start and then bigger numbers later on", and, to be honest, that isn't far off. But the devils in the details! I found when just guess-timating and semi-randomly assigning costs and earnings, with the idea of small numbers to start and big numbers later on in my head, that I ended up with inconsistent pacing; periods where players flew through buying new guys; but, more importantly, gaps where players weren't able to afford any new guys for way too long and got bored. <i>Bored!</i> Uh oh!<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packwithoutcost.png" width="192" height="192"><br/>
  <sup><i>how much should a guy cost?</i></sup><br/><br/>
</p>

  Of course, I didn't expect to land on the right numbers straight away - I'm a game developer after all! I was playtesting the game early and often for this very reason! From watching people play, I could see where the pacing problems occurred. Then, I just had to adjust the cost or earnings of any guys that slowed the pace too much - bring the costs down or push the earnings up to shorten the time till the next new guys. Done! Well, maybe. The tried and true, iterative, playtest and adjust approach worked but was slow. Whenever I added new guys I would guess-timate then have to play myself or even watch someone else play to know if the cost/earnings for the new guys didn't create too much downtime. "Surely there is a better approach?", you ask? "MATH!", I shout right back.<br/><br/>

  Anyone who has been around games with number driven progression for awhile can guess that there is some kind of funky, behind the scenes sum going on. Whether it's XP requirements to hit the next level in an RPG or the cost of the next upgrade in an idle game, the numbers look too random yet specific to have been guess-timated and tweaked by a developer. I knew this, but what kind of funky, behind the scenes sum was needed? Something that can be made into a graph, probably:<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/costvsearnings.png" width="192" height="192"><br/>
  <sup><i>bear with me</i></sup><br/><br/>
</p>

  Let me explain the graph. I wanted players to gain a solid collection of guys pretty quickly out the gate but I was happy for new guys to be acquired less often as time went on. Later in the game players would have lots of guys already, to play and experiment with, and I wanted to make sure there was room for that. Meaning, not only should the cost/earnings of guys start small and end big, but that the difference between the cost and earnings of guys should get larger as the game went on. This allows for more downtime (meaning time to play and experiment)! So, the cost and earnings of each new guy should diverge over time - and that's exactly what this graph represents!<br/><br/>

  I should say - none of this is groundbreaking! Most progression systems hit the gas and reward the player more regularly at the beginning of a game before slowly easing off the gas once the player is settled in. Look at Clash of Clans, a master of "number go up" progression, and the difference in time to upgrade it's townhall to level 2 vs level 17:<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/clashofclans.png" width="192" height="192"><br/>
  <sup><i>they make their players wait 103,680 times longer (!) for their last upgrade vs their first</i></sup><br/><br/>
</p>

Now I'd learned math, I could make the numbers go up the right way. At first, I went straight to programming in the graph required, having code drive the costs/earnings of guys entirely, and taking away all editor control. Using the order of guys, I could easily find their cost/earnings:<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/costvsearningsmarked.png" width="192" height="192"><br/><br/>
</p>

Quickly, however, I found out that although the pacing felt better, there were still inconsistencies and gaps where waits for new guys were too long. Entirely relinquishing control to code was not the right answer. Instead, I switched to using my graph/math output as a jumping off point for the costs/earnings of each new guy but still adjusting and having full control of the numbers from there. That way I didn't need to guess-timate anymore (nice!) but could still iterate and adjust based on playtest feedback (double nice!). At the end of the day, this was the approach that worked for me and my game. Not the most sophisticated but more than good enough!<br/><br/>

On reflection, I am happy with the way everything turned out. As my first attempt at making a progression system this large, I think it's reasonably good - although of course there is room for improvement. My biggest take away was the "combined" approach of using math/a graph as a jumping off point for costs etc before tweaking and adjusting through playtesting. It is an approach that rings true in a lot of game development I think. Taking the time to outline what you are trying to achieve with a design, creating a technical solution or approach that can approximate that, before finalising the design through playtesting and tweaking. I realise I am only scratching the surface of making a game's progression satisfying - if you know of any articles/videos/resources that expand on (or even make points against!) these ideas, please send them my way!<br/><br/>
</p>
