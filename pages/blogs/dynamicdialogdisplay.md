---
layout: post
title: 'Dynamic Dialog Display'
author: Thomas Wood
tagline: 'Dynamic Dialog Display'
description: Dynamic Dialog Display
---

<p align="left">
  When I started making <a href="https://twood27897.github.io/pages/whale-game.html">Remote Working</a> I knew I wanted to have characters speak to the player, saying things like "EXPOSITION!" and "Here's a hint at what you, the almighty player, should do next!". I wanted dialog! An issue I have, however, with the way dialog is displayed in a lot of games is that it only exists in a static, boring UI overlay/window. A box pops up at the bottom of the screen to click through with text detailing what is being said and as you hit whatever button is necessary to skip through the boxes you feel less like you are talking to someone and more like you just want to get it over and done with and get back to the gameplay. With that in mind, I wanted to take a different approach to displaying the dialog in my game. One that felt more fun. And that didn't immediately remind me of all the other times I'd been playing a game and had to wade through static boxes clogging up the bottom half of my screen. Introducing the Dynamic Dialog Display! In this blog, I am going to briefly describe what the Dynamic Dialog Display is and then get into some of the code I wrote to implement it. I faced plenty of design challenges while creating the Dynamic Dialog Display but for this blog I am going to purely focus on the engineering that went into getting everything working. Buckle in...<br/><br/>

To start, a description. My Dynamic Dialog Display is based in cartoon-style speech bubbles. Rather than the static box, I display dialog in the game world, connected to a character but able to move around as the player moves or looks around. Similar displays can be found in games like <i>A Short Hike</i> or <i>Oxenfree</i> but I wanted to push mine in a slightly different direction. As most of the 3D games I make are first person, I wanted to allow for the player to walk and/or look away from a character as they are speaking. The display would need to handle this, and it can. If a character is speaking outside the player's field of view a speech bubble will still appear; it's "point" at the edge of the screen in the direction of the character. This allows for players to retain agency even when a character is speaking (something that is not afforded by most dialog display systems!). A player can look around or even walk away mid sentence - pretty cool if you ask me! Enough text description for now though: as the saying goes, two gifs are worth a thousand words, so here are two gifs of the current version of my Dynamic Dialog Display:<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>my dialog display in action</i></sup><br/><br/>
</p>
<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>the display handling the player walking/looking away</i></sup><br/><br/>
</p>

  And with that, onto the implementation! To start, we need some data. The pieces of information required for the dialog display system to work. The first is common to all dialog displays - it needs the dialog! A piece of text/speech that will be shown inside the display - something like "Over here!" or "Will you take on my heroic quest?". The second is not common to all dialog displays but is required for mine: the location of the speaker. Or more importantly the transform of the speaker. This will be used to decide where to display the dialog on the screen (and by caching the transform instead of the raw position we can keep up to date if the speaker moves around and their position changes). Knowing these two pieces of required data, here is a simple function to display some dialog (all code snippets are in C# using the Unity engine as that is what I used to make the system):<br/><br/>

<code>public void AddDialog(string speakerText, Transform speakerTransform)<br/>{<br/>// Some cool code to display the dialog<br/>// Keep reading to find out what it is!<br/>}</code><br/><br/>
  
  After we have the data needed to get on with displaying the dialog, there are two main things to think about: 1. The body of the speech bubble and: 2. The little point-y bit that comes off the speech bubble. Each needs to be positioned correctly (and the point needs to be rotated correctly too!). I will talk about each separately:<br/><br/>
  
  <b><i>The Body Of The Speech Bubble</i></b><br/><br/>
To start with the display proper, I created an object that contained a text display element and a background element. This represents the body of the speech bubble. I created one of these objects whenever new dialog was added and call it's <code>Open()</code> function, setting it up to be display:<br/><br/>

<code>public void AddDialog(string speakerText, Transform speakerTransform)<br/>{<br/>GameObject dialogObject = Instantiate(dialogTemplate, transform);<br/>dialogObject.GetComponent&lt;Dialog&gt;().Open(speakerText, speakerTransform);<br/>}</code><br/><br/>

But where will the dialog appear? How will it know where to go on the screen? To keep things simple at the start, I just positioned the body of the speech bubble at the position of the speaker, using the <code>Open()</code> function for the initial setup and then the <code>Update()</code> function to keep the position correct as time passes:<br/><br/>

<code>public void Open(string speakerText, Transform speakerTransform)<br/>{<br/>dialogText.text = newText;<br/><br/>transformToFollow = newTransform;<br/>transform.position = Camera.main.WorldToScreenPoint(transformToFollow.position);<br/>}<br/><br/>void Update()<br/>{<br/>transform.position = Camera.main.WorldToScreenPoint(transformToFollow.position);<br/>}</code><br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>here's what that looks like in action</i></sup><br/><br/>
</p>

Off to a good start, the dialog appears! And vaguely resembles a speech bubble! If I walk away and the speaker moves out of view though, the dialog goes with it. To stop that happening I need to know where the speak is on screen and then accommodate for them going off screen, keeping the speech bubble on the edge of the screen closest to the speaker. By translating the speaker's position into viewport space <a href="https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Camera.WorldToViewportPoint.html">(reference to relevant Unity docs here)</a> I can do just that! Or at least the first half! If the speaker viewport position is less than 0 or greater than 1 in the x, y or z axis it is off screen! Now, to get the speech bubble to stay on the edge of the screen when the speaker is not visible. To do this, I use the viewport position to get the normalized direction to the speaker's position from the centre of the screen. I can then multiply this normalized direction by the distance from the centre of the screen that I want the speech bubble to sit at - giving me a new viewport position that makes it look like the speech bubble clamps to the edge of the screen! There is an implementation detail that is important here though. The viewport position goes from 0 to 1 starting at the bottom left corner of the screen and ending at the top right. To use it as a direction from the centre of the screen I must take away 0.5 from both the x and y components. This means that a viewport position of (0, 0) that previously would have had no direction changes to (-0.5, -0.5) and denotes a direction pointing to the bottom left of the screen which is what I want. Here's the code I ended up with, having pulled everything into a <code>CalculatePosition()</code> function so <code>Open()</code> and <code>Update()</code> can both call it:<br/><br/>

<code>private Vector3 CalculatePosition()<br/>{<br/>Vector3 viewportPosition = Camera.main.WorldToViewportPoint(transformToFollow.position);<br/><br/>// We are ignoring the z position for now<br/>viewportPosition.z = 0.0f;<br/><br/>Vector3 directionFromCentre = viewportPosition - new Vector3(0.5f, 0.5f, 0.0f);<br/>Vector3 distanceFromCentre = directionFromCentre.normalized * distanceToEdgeOfScreen;<br/>Vector3 adjustedPosition = new Vector3(0.5f, 0.5f, 0.0f) + distanceFromCentre;<br/><br/>return Camera.main.ViewportToScreenPoint(adjustedPosition);<br/>}</code><br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>our speech bubble now moves to the edge!</i></sup><br/><br/>
</p>

But wait! Now the speech bubble is always at the edge of the screen. That isn't very nice. To fix this I created a "buffer zone" in the centre of the screen. If the speaker's position is within this zone, I don't move it towards the edge of the screen at all:

<code>private Vector3 CalculatePosition()<br/>{<br/>Vector3 viewportPosition = Camera.main.WorldToViewportPoint(transformToFollow.position);<br/><br/>// We are ignoring the z position for now<br/>viewportPosition.z = 0.0f;<br/><br/>bool inCentreOfScreen = viewportPosition.x > 0.25f && viewportPosition.x < 0.75f && viewportPosition.y > 0.25f && viewportPosition.y < 0.75f;<br/>if (inCentreOfScreen)<br/>{<br/>return Camera.main.ViewportToScreenPoint(viewportPosition);<br/>}<br/>else<br/>{<br/>Vector3 directionFromCentre = viewportPosition - new Vector3(0.5f, 0.5f, 0.0f);<br/>Vector3 distanceFromCentre = directionFromCentre.normalized * distanceToEdgeOfScreen;<br/>Vector3 adjustedPosition = new Vector3(0.5f, 0.5f, 0.0f) + distanceFromCentre;<br/>return Camera.main.ViewportToScreenPoint(adjustedPosition);<br/>}<br/>}</code><br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>looking better again</i></sup><br/><br/>
</p>

Although the speech bubble is now positioning itself correctly it can still move offscreen sometimes. This is because the position doesn't account for the width of the speech bubble itself - it just positions the centre of the speech bubble to where it's told and that's it. To get the speech bubble to orientate itself towards the centre of the screen, I used it's pivot. By changing the speech bubbles pivot, it will orient itself differently around it's position: with a pivot of (0,0) the speech bubble will put it's bottom left corner where it's position is, with a pivot of (1.0, 1.0) the speech bubble will put it's top right corner where it's position is etc. This means I can change the pivot of the speech bubble so that when it's on the left hand side of the screen, it puts it's left edge where it's position is, and when it's at the top of the screen, it puts it's top edge where it's position is, and so on. I also added a little extra to the x and y of the pivot so that the speech bubble will sit slightly further into the screen instead of being exactly on the edge:<br/><br/>

<code>private Vector2 CalculatePivot()<br/>{<br/>Vector3 viewportPosition = Camera.main.WorldToViewportPoint(transformToFollow.position);<br/><br/>// We are ignoring the z position for now<br/>viewportPosition.z = 0.0f;<br/><br/>bool inCentreOfScreen = viewportPosition.x > 0.25f && viewportPosition.x < 0.75f && viewportPosition.y > 0.25f && viewportPosition.y < 0.75f;<br/>if (inCentreOfScreen)<br/>{<br/>return new Vector2(0.5f, 0.5f);<br/>}<br/>else<br/>{<br/>Vector3 targetViewportPosition = viewportPosition;<br/>targetViewportPosition.x = Mathf.Clamp(targetViewportPosition.x, -0.025f, 1.025f);<br/>targetViewportPosition.y = Mathf.Clamp(targetViewportPosition.y, -0.1f, 1.1f);<br/><br/>float distanceToCentreOfScreen = (viewportPosition - new Vector3(0.5f, 0.5f, 0.0f)).magnitude - 0.25f;<br/><br/>return Vector2.Lerp(new Vector2(0.5f, 0.5f), targetViewportPosition, distanceToCentreOfScreen / 0.5f);<br/>}<br/>}</code><br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>trapped forever, the dialog will never leave the screen!</i></sup><br/><br/>
</p>

The eagle-eyed reader might have noticed that I have been ignoring the z position! Well, that stops here! Ignoring the z position up until now has meant that when the player entirely turns away from the speaker, and is facing the opposite direction, the dialog display thinks the speaker is in fact still in view:<br/><br/>

<p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>something doesn't look right</i></sup><br/><br/>
</p>

To fix this I adjust the viewport position before doing any of the other calculations, exactly how I have been zeroing out the z axis of the viewport position up until now. The extra adjustments I need to make are to the x and y axis. When the player has their back turned, the viewport's x axis returns within a 0 to 1 range as if the player was facing the speaker. This is because, unless we consider the depth (i.e. z axis), the speaker is still within the viewports width. To handle this I check if the z axis of the viewport position is less than 0 (meaning the player's back is turned) and then clamp the x axis position to 0 or 1 depending on which side of the player the viewport position is on. The y axis has the same problem. When the player's back is turned but the speaker is within the viewports height then the y axis of the viewport position returns within a 0 to 1 range. Different to the x axis though I don't want to clamp the y axis to 0 or 1. Instead, I allow the y axis to move through the 0 to 1 range so that the speech bubble moves to the top/middle/bottom of the screen to show which way the dialog is coming from. When the z axis of the viewport position is negative, however, the y axis of the position is inverted so I invert it back to keep the y position of the speech bubble consistent:<br/><br/>

<code>private Vector3 AdjustViewportPosition(Vector3 viewportPosition)<br/>{<br/>if (viewportPosition.z < 0.0f)<br/>{<br/>viewportPosition.x = viewportPosition.x >= 0.5f ? 0.0f : 1.0f;<br/><br/>viewportPosition.y = 1.0f - viewportPosition.y;<br/>}<br/>viewportPosition.z = 0.0f;<br/><br/>return viewportPosition;<br/>}</code><br/><br/>

And with that, I have the speech bubble's body positioning itself in a somewhat reasonable way! But I'm not done yet. I still need to give the speech bubble a little point that goes in the right direction.<br/><br/>
    
  <b><i>The Point Of The Speech Bubble</i></b><br/><br/>

  To start, I added an image element to my speech bubble object. I gave this image element a little triangle sprite so that it could represent the point. Now I just need to make it point in the right direction! To do this, I calculate the direction from the point's position on the screen, to the speaker's position. Making the point rotate to match this direction makes it point at the speaker:

  <code>private Quaternion CalculatePointRotation()<br/>{<br/>Vector3 viewportPosition = GetAdjustedViewportPosition(transformToFollow.position);<br/><br/>Vector3 pointViewportPosition = Camera.main.ScreenToViewportPoint(dialogBoxPoint.position);<br/>pointViewportPosition.z = 0.0f;<br/><br/>Vector3 pointDirection = (viewportPosition - pointViewportPosition).normalized;<br/><br/>return Quaternion.FromToRotation(Vector3.up, pointDirection);<br/>}</code><br/><br/>

  <p align="center">
  <img src="https://twood27897.github.io/assets/packathon.gif" width="256" height="192"><br/><sup><i>the point rotates!</i></sup><br/><br/>
</p>


</p>
