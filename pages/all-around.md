---
layout: post
title: 'ALL AROUND'
author: Thomas Wood
tagline: 'A game I made for PC in which you click to stop yourself from losing lives while trying to hit enemies and miss friends'
description: A game I made for PC in which you click to stop yourself from losing lives while trying to hit enemies and miss friends
---

<img src="https://twood27897.github.io/allaroundpalettechange.gif" width="382" height="215">

All Around is a game where you try to destroy as many enemy circles as possible while trying to avoid destroying friendly circles. The main focus of this project for me was the visuals, I wanted to create a game with a definitive style and incorporate graphics programming in some way. 

To create the pixel aesthetic of the game I use a render texture from a camera sharing the exact position of the main camera. I then downsize the texture and display it on a quad. For the texture to maintain solid blocks of colour I avoid bi/trilinear filtering and stick to a point filter. Here is a before and after of the game without then with the downsizing technique:

<img src="https://twood27897.github.io/assets/allaroundnofilter.gif" width="252" height="142">
<img src="https://twood27897.github.io/assets/allaroundfilter.gif" width="252" height="142">
