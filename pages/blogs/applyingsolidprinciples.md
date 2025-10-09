---
layout: post
title: 'Applying SOLID Design Principles'
author: Thomas Wood
tagline: 'Applying SOLID Design Principles'
description: Applying SOLID Design Principles
---

<p align="left">
  In object-oriented programming it is easy to make a mess. Classes can become bloated and reach incomprehensible sizes that would make any software engineer blush. It happens. For real, it happens. To people. Even to people who are writing blogs about it that you are reading right now. While developing my own games, I have largely favoured moving quickly and breaking stuff over writing clean, maintainable, extensible, non-messy code. When working on your own and prototyping ideas this can be a great approach! After all, any time you spend perfecting a system's architecture could be wasted as the design of what you're working on inevitably changes and you things throw out. Eventually though there comes a time where you know a system is here to stay. For example, I have now used the same interaction system in three of my projects. It is time for me to make the code comprehensible and to clean up my mess. To do that I am going to apply <a href="https://en.wikipedia.org/wiki/SOLID">SOLID</a> design principles.<br/><br/>

  When I have worked in teams in the past, SOLID has been a great way to architect large systems. A go to approach that pushes you to breakdown your code into sizeable chunks while also reducing dependencies, SOLID is pretty spot on for helping teams to write and maintain large codebases. The first principle of SOLID is the <i>Single Responsibility Principle</i>. Each class you create should only have one responsibility. Just by giving my interaction system's <i>Interactor</i> class a side ways glance I can tell it has more than one responsibility. In fact, it has three real big ones:<br/>
  
  <ol type="1"><li>Targeting interactable objects</li><li>Triggering interactions with interactable objects</li><li>Visualising available interactable objects</li></ol><br/>

  
</p>
