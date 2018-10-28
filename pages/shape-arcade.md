---
layout: post
title: 'SHAPE ARCADE'
author: Thomas Wood
tagline: 'A game I made for Android and PC my first summer of university. It holds a special place as my first personal project outside of game jams'
description: A game I made for Android and PC my first summer of university. It holds a special place as my first personal project outside of game jams
---

<p align="center">
  INSERT INITIAL IMAGE<br/><br/>
  
  Shape Arcade is a collection of four highscore minigames where the player only ever needs to tap the screen. My main goals for the
  project were to complete a game and release it on mobile and PC to gain a better understanding of the full development cycle on a
  smaller game. Now looking back I can see major points of improvement and how I could have done things a lot better from the outset.<br/><br/>
  
  Putting the game on multiple platforms wasn't a massive challenge technically as I developed the game in Unity, which very easily
  allows for porting to multiple platforms. Instead the main challenge was in establishing different user inputs across different
  devices. While usually on PC I had no problem accessing key presses now I had to deal with touch input and try and figure out how to
  read a swipe from the player. This is how I did it:<br/><br/>
</p>

```
  // Get current touch
  Touch touch = Input.GetTouch(0);
    
  // Current touch just began
  if (touch.phase == TouchPhase.Began)
  {
    // Save initial position of touch
    firstPressPos = touch.position;
  }
  // Current touch just ended
  if (touch.phase == TouchPhase.Ended)
  {
    // Take end point of touch
    secondPressPos = touch.position;
      
    // Find direction between beginning and end of touch and normalize it
    currentSwipe = secondPressPos - firstPressPos;
    Vector2 currentSwipeNormalized = currentSwipe.normalized;
      
    // Find one tenth of screen width and height
    float w = Screen.width / 10;
    float h = Screen.height / 10;
      
    // Make checks between touch size and screen w and h
    // Check direction of touch if large enough
    // Execute appropriate action based on outcome
  }
```
<p align="center">
  Early testing and research meant I very quickly became aware of one of the main hurdles in mobile development - non-uniform devices
  sizes and capabilities. Pulling the screen height and width and using a portion of that allowed me to accommodate for swipes on
  multiple sizes of device as it seems people tend to make bigger swipes on bigger screens and vice versa for smaller devices.
  
  The rest of the project was pretty straight forward. Checking the scale, rotation and position of sprites against each other per
  minigame and then reseting and adding speed if they are similar. 
</p>

[https://github.com/twood27897/Shape-Arcade](https://github.com/twood27897/Shape-Arcade)<br/>
[https://helloimtw.itch.io/shape-arcade](https://helloimtw.itch.io/shape-arcade)<br/><br/>
