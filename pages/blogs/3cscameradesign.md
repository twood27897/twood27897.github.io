---
layout: post
title: 'The 3Cs: Camera (Design)'
author: Thomas Wood
tagline: 'The 3Cs: Camera (Design)'
description: The 3Cs: Camera (Design)
---

<p align="left">
  <i>"The 3Cs of Game Development"</i> - Character, Controls, Camera. This is a term that I've seen pop up here and there in my years doing game development. Some combination of Character, Controls and Camera usually make up the core interaction system of any video game whether it's a Realtime Stategy Game or a Third Person Shooter. This means The 3Cs are something to nail down early in development. You want to make sure you have a good idea of what the central character(s), controls and camera(s) in your game look like before leaving prototyping and entering production. The <a href="https://pixelatron.com/blog/the-making-of-super-mario-64-full-giles-goddard-interview-ngc/">stories of Mario 64's early development</a> only being concerned with perfecting Mario's controls, camera, animations etc. (and Miyamoto spending 99% of his time on it during development) come to mind. The 3Cs are important! So, with that in mind, I thought I'd spend some time designing and implementing a version of the 3C that I have spent the least amount of time on so far in my development career: the Camera!<br/><br/>
  
  <i>Note: Not only have I seen reference to "The 3Cs of Game Development" but also the 4Cs (Choice, Chance, Change, Challenge). See my blog <a href="https://helloimtw.me/pages/blogs/sela.html">SELA (Software Engineers Love Acronyms)</a> for even more acronym fun! On review, another blog tiled "AGDELA (All Game Developers Everywhere Love Acronyms)" or maybe even "HELA (Humans Everywhere Love Acronyms)" might be in order!</i><br/><br/>

  To get the most out of designing and implementing a camera, I have decided to split myself in two. First, I am going to put my games designer hat on. I am going to work through a simple, light-weight design process, starting with a feature brief, then researching and analysing other successful designs, before creating a feature design doc outlining what I need from the camera including goals and requirements. Once that's done, I am going to put on my gameplay programmer hat! I'll take the feature design doc and implement a camera system that achieves the goals and meets the requirements. I don't want to get bogged down too much along the way though, as I know that a lot of learnings will come from iteration as the camera system is being implemented. I am going to move quickly and hit the ground running! To start, I am going to use this blog to cover my design process (and then follow it up with another blog detailing my implementation next week).<br/><br/>

  <b>Feature Brief</b><br/><br/>

  Here is my made up feature brief for my camera:<br/><br/>

  <i>"For our new third person action game we need a dynamic camera. We want to gives players front row seats as they traverse and fight their way through the game world. We don't need to worry about the camera handling cinematics at this stage, this camera will purely be focused on gameplay moments."</i><br/><br/>

  Forgive a little of the vagueness, I didn't want to get caught up in coming up with a whole game idea for this exercise so I thought I'd keep the brief high level and slightly more abstract than it might be in a real game development setting.<br/><br/>
  
  <b>Research and Analysis</b><br/><br/>

  From the feature brief, I can begin researching. I know I am going to be looking at third person action games, probably the best in class, and analysing how they have designed their cameras. I am going to look specifically at combat and traversal moments. By focusing on these, I should get a pretty good understanding of what goes into a good third person camera system, and what I'll need to meet my feature brief. I am not going to spend too long on any single game but instead keep a broader view, doing quick dives and very brief visually driven case studies (with images and links to videos) to highlight specific details that are relevant to the feature I am researching.<br/><br/>

  Rather than write about my research and analysis in detail here, I have created a mural board that contains the research I did which you can find <a href="https://app.mural.co/t/asfadsf8488/m/asfadsf8488/1761920395146/77602e8713fa712ec7d742173beeac191712cb6d?sender=uc97fc6a5de48809d05dc4099">here</a>.<br/><br/>

In short, I looked at a number of third person games to see what their cameras did, delving more deeply into a select few:<br/><br/>

<ol type="2"><li><i>God of War: Ragnarok</i> as it is well known for it's cinematic nature</li><li><i>The Last of Us: Part 2</i> for similar reasons</li><li><i>Jedi Survivor</i> as it handles a larger and more dynamic traversal set than most third person action games</li><li><i>Zenless Zone Zero</i> because (without getting on my soapbox too much here) I think there is a lot to learn from MiHoYo games and the way they stylise and centre action and animation</li></ol><br/>

   From my research and analysis, a few things jumped out. First, cameras in third person games can be very dynamic. Often there is a new camera movements/angles as the player interacts and traverses through the world. Second, camera control is very rarely taken away from the player outside of cutscenes. The only exception here is for quick snappy camera movements that go alongside moments like opening a chest or hitting a finishing move on an enemy. The last thing I'll touch on here is post processing/overlays. Across every game I looked at I found post processing effects that changed depending on what the player did, the most common example was applying a vignette when the player aims a weapon. The most impressive of all these post processing effects was <i>The Last of Us</i>'s blood splatter post process, which seemingly only applied if the camera was in the way of the blood vfx and also reacted to the light. Game cameras physically grounded in real life camera science have never come so far!<br/><br/>
  
  <b>Feature Design Doc</b><br/><br/>

  Now I feel like I've done enough analysis, I'll move onto creating a Feature Design Doc. The main objective here is to create a document that outlines everything that is needed from the camera by drawing from the Feature Brief and my research. Once the Feature Design Doc is done, I can then use it as a starting off point for implementation by sharing it with whoever will be working on it (in this case, me wearing another hat). There's lots of ways to structure a Feature/Game/Product/Any Design Doc. I'm going to keep mine as simple as possible. This is usually my preference, as designs usually change over time as you iterate, and keeping it simple helps alleviate headaches as chunks of over detailed design work might be made redundant as the feature progresses.<br/><br/>

  Similarly to my research and analysis, I am not going to detail all of my Feature Design Doc in this blog. Instead you can find it <a href="https://app.mural.co/t/asfadsf8488/m/asfadsf8488/1761929908894/9bf195df9b89447f459d76aec7f847a13123a035?sender=uc97fc6a5de48809d05dc4099">here</a>.
</p><br/><br/>

The Feature Design Doc is split into two sections: one for Goals and another for Gameplay Scenarios. The Goals are fairly self explanatory. A high level, concise, what does this feature want to achieve list. The Gameplay Scenarios are player centered stories that uphold the goals. These are less high level, things like <i>"As I run the camera pulls out and adds a vignette"</i>. Below is the Goals section from the doc, I won't include the Gameplay Scenarios section in the blog as it is larger but if you want to see it you can look in the doc <a href="https://app.mural.co/t/asfadsf8488/m/asfadsf8488/1761929908894/9bf195df9b89447f459d76aec7f847a13123a035?sender=uc97fc6a5de48809d05dc4099">here</a>.<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/camera/cameragoals.png" width="822" height="472"><br/><sup><i>GOOOOAAAAALLLL(s)</i></sup><br/><br/>
</p>

  <b>Conclusion</b><br/><br/>

  With a complete Feature Design Doc, I can now start implementation on my camera. My plan is to spend the next week as my Gameplay Programmer self, whipping up systems to create the Gameplay Scenarios listed and meet the Goals required, before writing a blog summarising the process!<br/><br/>

  <i>P.S. Happy Halloweeeeeeeeeeeen! (as of finishing this blog I am about to head out for a very spooky night!)</i>

