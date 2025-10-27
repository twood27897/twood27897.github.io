---
layout: post
title: 'Dreams in The Whale Game'
author: Thomas Wood
tagline: 'Dreams in The Whale Game'
description: Dreams in The Whale Game
---

<p align="left">
  My latest project <a href="https://helloimtw.me/pages/whale-game.html">The Whale Game</a> is a 3D puzzle room centered on a 2D incremental game. The game is structured into days - within which the player wakes up to do their job, plays the 2D incremental game, tries to uncover the mysteries of the 3D puzzle room and then goes to sleep. Rinse and repeat. Each time the player goes to sleep, however, there is a chance they will be presented with a dream sequence. In this blog I want to talk about why I chose to put the dream sequences into the game and also share some of the code that powers them.<br/><br/>

  First, why. Early in development of The Whale Game, I focused on the core mechanics and systems. I implemented the 2D incremental game and a version of the 3D puzzle room alongside additional systems I knew I needed. Players could receive emails on a computer (delivering narrative and light tutorialisation), they could earn money by doing their day job and they could spend that money in the 2D incremental game. Through playtesting early in development though, two big issues presented themselves:<br/><br/>

   <ol type="1"><li>Staying in the same room (even for a short time) felt overly repetitive when spread across a number of in game days</li><li>Text based emails were likely to be skipped over or misread by players who didn't engage with reading</li></ol><br/>

   While both these issues stemmed partly from the game being early in development, I decided to approach my solving of them more holistically. Make the emails more punchy and entertaining, make the room more interesting and engaging but at the same time add a new feature to the game to more directly balance the player experience. Attack the problem on all sides. Enter the new feature: dreams.<br/><br/>

But when I say dreams, what do I mean? In The Whale Game, dreams are levels/sequences that take place when the player goes to bed. They are short and involve the player interacting with characters and locations outside of the 3D puzzle room. Dreams can deliver narrative as dialog or through new mechanics while also breaking up the pace of the game so it doesn't feel as repetitive. I intentionally have put very few hard rules and limits on the dreams in the game. They provide a structure for a variety of ideas and can be used to solve a lot of different problems. The main rule I keep in mind when designing dreams is: keep them short! While they can engage the player for a long time, that must be a choice the player makes. If the player wants to get back to the main game quickly, they should be able to. It was also apparent to me early on that dreams must not appear randomly. For them to be able to deliver narrative, they must be able to be sequenced with other dreams or even with gameplay moments. A dream might only take place after another or after the player has achieved something in the game world. Dreams must have conditions under which they appear.<br/><br/>

With all this in mind, I quickly built a dreams system with three main parts:<br/><br/>

<ol type="1"><li><b>a <i>Dreams</i> script</b> that manages a list of which dreams are available and has a function called <i>Dream()</i> that will trigger whichever dream is at the top of the list</li>
<li><b>a <i>Dream</i> scriptable object type</b> containing the name of the scene a dream takes place in and the conditions under which that dream is available/can be triggered. Using a scriptable object to represent each dream allows new ones to be easily created in editor. Simply make a new scene with whatever is needed for the dream and then add that scene's name and whichever conditions are required to a new <i>Dream</i> scriptable object and you're off to the races!</li>
<li><b>a <i>DreamCondition</i> scriptable object type</b> defining a condition that must be met for a dream to be considered available by the <i>Dreams</i> script. By making each <i>DreamCondition</i> a scriptable object, I can write a single script for a new type of condition and then easily create multiple versions of that condition with different parameters to use with different dreams. For example, if some dreams are only made available after a certain amount of time has passed, I can create a <i>TimePassedDreamCondition</i> script with a parameter for how much time needs to have passed and then have a version of it for fives minutes passing and a version for one day passing and attach each of those conditions to different dreams. This flexibility enables faster iteration</li></ol><br/>

Since implementing the dreams system, not much has changed about it. Most of the complexity of implementation lies within the content of the dreams themselves. What takes place inside each dream? Are there new mechanics that need implemented? What common patterns/recurring dreams exist and how can they be abstracted/reused? I won't dive into the answers to all these questions here. Overall though it's been easy to see which parts of the game could be supported by a dream and in general I've found dreams to be succesful in fulfilling their original goals. The game feels less repetitive and the narrative delivery has been balanced away from text heavy emails, dreams have allowed for new characters and settings to be introduced while the player remains in one location.<br/><br/>

Take a look at the code: <a href="https://github.com/twood27897/Dreams-in-The-Whale-Game">github.com/twood27897/Dreams-in-The-Whale-Game</a><br/>
</p>
