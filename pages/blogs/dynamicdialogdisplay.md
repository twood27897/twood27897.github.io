---
layout: post
title: 'Dynamic Dialog Display'
author: Thomas Wood
tagline: 'Dynamic Dialog Display'
description: Dynamic Dialog Display
---

<p align="left">
  When I started making <a href="https://twood27897.github.io/pages/whale-game.html">The Whale Game</a> I knew I wanted to have characters speak to the player, saying things like "EXPOSITION!" and "Here's a hint at what you, the almighty player, should do next!". I wanted dialog! An issue I have, however, with the way dialog is displayed in a lot of games is that it only exists in a static, boring UI overlay/window. A box pops up at the bottom of the screen to click through with text detailing what is being said and as you hit whatever button is necessary to skip through the boxes you feel less like you are talking to someone and more like you just want to get it over and done with and get back to the gameplay. With that in mind, I wanted to take a different approach to displaying the dialog in my game. One that felt more fun. And that didn't immediately remind me of all the other times I'd been playing a game and had to wade through static boxes clogging up the bottom half of my screen. Introducing the Dynamic Dialog Display! In this blog, I am going to briefly describe what the Dynamic Dialog Display is and then get into some of the code I wrote to implement it. I faced plenty of design challenges while creating the Dynamic Dialog Display but for this blog I am going to purely focus on the engineering that went into getting everything working. Buckle in...

To start, a description. My Dynamic Dialog Display is based in cartoon-style speech bubbles. Rather than the static box, I display dialog in the game world, connected to a character but able to move around as the player moves or looks around. Similar displays can be found in games like <i>A Short Hike</i> or <i>Oxenfree</i> but I wanted to push mine in a slightly different direction. As most of the 3D games I make are first person, I wanted to allow for the player to walk and/or look away from a character as they are speaking. The display would need to handle this, and it can. If a character is speaking outside the player's field of view a speech bubble will still appear; it's "point" at the edge of the screen in the direction of the character. This allows for players to retain agency even when a character is speaking (something that is not afforded by most dialog display systems!). A player can look around or even walk away mid sentence - pretty cool if you ask me! Enough text description for now though: as the saying goes, two gifs are worth a thousand words, so here are two gifs of the current version of my Dynamic Dialog Display:

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>my dialog display in action</i></sup><br/><br/>
</p>
<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>the display handling the player walking/looking away</i></sup><br/><br/>
</p>

  And with that, onto the implementation! To start, we need some data. The pieces of information required for the dialog display system to work. The first is common to all dialog displays - it needs the dialog! A piece of text/speech that will be shown inside the display - something like "Over here!" or "Will you take on my heroic quest?". The second is not common to all dialog displays but is required for mine: the location of the speaker. Or more importantly the transform of the speaker. This will be used to decide where to display the dialog on the screen (and by caching the transform instead of the raw position we can keep up to date as the speaker might move around and their position might change). Knowing these two pieces of required data, here is a simple function to display some dialog (all code snippets are in C# using the Unity engine as that is what I used to make the system):<br/><br/>

<code>public void AddDialog(string speakerText, Transform speakerTransform)<br/>{<br/>// Some cool code to display the dialog<br/>// Keep reading to find out what it is!<br/>}</code><br/><br/>
  
  After we have the data needed to get on with displaying the dialog, there are two main things to think about: 1. The body of the speech bubble and: 2. The little point-y bit that comes off the speech bubble. Each needs to be positioned correctly (and the point needs to be rotated correctly too!). I will talk about each separately:<br/><br/>
  
  <b><i>The Body Of The Speech Bubble</i></b><br/><br/>
To start with the display proper, I created an object that contained a text display element and a background element. This represents the body of the speech bubble. I created one of these objects whenever new dialog was added and call it's <code>Open()</code> function, setting it up to be display:<br/><br/>

<code>public void AddDialog(string speakerText, Transform speakerTransform)<br/>{<br/>GameObject dialogObject = Instantiate(dialogTemplate, transform);<br/>dialogObject.GetComponent<Dialog>().Open(speakerText, speakerTransform);<br/>}</code><br/><br/>

But where will the dialog appear? How will it know where to go on the screen? To keep things simple at the start, I just positioned the body of the speech bubble at the position of the speaker, using the <code>Open()</code> function for the initial setup and then the <code>Update()</code> function to keep the position correct as time passes:<br/><br/>

<code>public void Open(string speakerText, Transform speakerTransform)<br/>{<br/>dialogText.text = newText;<br/><br/>transformToFollow = newTransform;<br/>transform.position = Camera.main.WorldToScreenPoint(transformToFollow.position);<br/>}<br/><br/>void Update()<br/>{<br/>transform.position = Camera.main.WorldToScreenPoint(transformToFollow.position);<br/>}</code><br/><br/>

  <b><i>The Point Of The Speech Bubble</i></b><br/><br/>
</p>
