---
layout: post
title: 'Shape Arcade Blog'
author: Thomas Wood
tagline: 'A game I made for Android and PC my first summer of university. It holds a special place as my first personal project outside of game jams'
description: A game I made for Android and PC my first summer of university. It holds a special place as my first personal project outside of game jams
---

<p align="center">
  <img src="https://twood27897.github.io/assets/shapearcademain.gif" width="491" height="292"><br/><br/>
  
  Shape Arcade is a collection of four highscore minigames where the player only ever needs to tap the screen. My main goals for the
  project were to complete a game and release it on mobile and PC to gain a better understanding of the full development cycle on a
  smaller game. Now looking back I can see major points of improvement and how I could have done things a lot better from the outset.<br/><br/>
  
  Putting the game on multiple platforms wasn't a massive challenge technically as I developed the game in Unity, which very easily
  allows for porting to multiple platforms. Instead the main challenge was in establishing different user inputs across different
  devices. While usually on PC I had no problem accessing key presses now I had to deal with touch input and try and figure out how to
  read a swipe from the player. This is how I did it:
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
  multiple sizes of device as it seems people tend to make bigger swipes on bigger screens and vice versa for smaller devices. The rest
  of the project was pretty straight forward. Checking the scale, rotation and position of sprites against each other per minigame and
  then reseting and adding speed if they are similar.<br/><br/>
  
  <img src="https://twood27897.github.io/assets/shapearcadecircle.gif" width="246" height="146">
  <img src="https://twood27897.github.io/assets/shapearcadetriangle.gif" width="246" height="146">
  <img src="https://twood27897.github.io/assets/shapearcadesquare.gif" width="246" height="146"><br/><br/>
  
  Now when I look through the project I can see a lot of ways to improve. Structually I could have streamlined everything by breaking it
  down into smaller chunks - functions, separate classes and objects. For example I recreate very similar code for ending each game
  individually when I could have very easily unified it. I also set different aspects of the transform manually accessing it each time
  when I could have set up functions for setting the scale, rotation and position of transforms in a tools class.<br/><br/>
  
  The biggest mistake I made with this project however was not using source control during development. I attempted to implement a
  highscore leaderboard using Google's Firebase SDK and got halfway through before hitting a wall that I couldn't get past and now the
  project needs a lot of work to get back to a place where it can be built upon. Source control would have allowed me to easily roll
  back to a version before I started this implementation and I could have had branches for the WebGL and Android builds as well as the
  Firebase implementation.<br/><br/>
  
  Moving forward I don't think I am going to put more time into this project. It's nice to leave it the way it is as a reminder of how
  far I've come and how much I've learnt. As my first completed game I am proud of it.
</p>

See the repository: [https://github.com/twood27897/Shape-Arcade](https://github.com/twood27897/Shape-Arcade)<br/>
Play the game: [https://helloimtw.itch.io/shape-arcade](https://helloimtw.itch.io/shape-arcade)<br/><br/>
