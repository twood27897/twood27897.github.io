---
layout: post
title: 'ALL AROUND'
author: Thomas Wood
tagline: 'A game I made for PC in which you click to stop yourself from losing lives while trying to hit enemies and miss friends'
description: A game I made for PC in which you click to stop yourself from losing lives while trying to hit enemies and miss friends
---

<p align="center">
  <img src="https://twood27897.github.io/assets/allaroundpalettechange.gif" width="382" height="215">

  All Around is a game where you try to destroy as many enemy circles as possible while trying to avoid destroying friendly circles. The     main focus of this project for me was the visuals, I wanted to create a game with a definitive style and incorporate graphics programming   in some way. 

  To create the pixel aesthetic of the game I use a render texture from a camera sharing the exact position of the main camera. I then       downsize the texture and display it on a quad. For the texture to maintain solid blocks of colour I avoid bi/trilinear filtering and       stick to a point filter. Here is a before and after of the game without, then with, the downsizing technique:

  <img src="https://twood27897.github.io/assets/allaroundnofilter.gif" width="382" height="215"> <img                                         src="https://twood27897.github.io/assets/allaroundfilter.gif" width="382" height="215">

  You may have noticed that the game is all greyscale in these two gifs and that is to allow for colour changing/palette swapping. To give   colour to the screen I use a shader which takes the red channel of the colour of each individual pixel and samples a palette texture with   it as the x-coordinate. The shader also allows for multiple palettes using a palette switching variable which can be changed on runtime     through scripts. This variable is mapped to the y-coordinate when sampling the palette texture per pixel. The palette texture and the       result of this shader can be seen below:

  <img src="https://twood27897.github.io/assets/palettes.png" width="400" height="300"> 
  <img src="https://twood27897.github.io/assets/allaroundpalettechange.gif" width="382" height="215">
</p>
