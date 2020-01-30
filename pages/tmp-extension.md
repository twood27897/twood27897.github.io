---
layout: post
title: 'TEXT MESH PRO EXTENSION'
author: Thomas Wood
tagline: 'I extended the TextMeshPro Unity package to include three new tags, which have runtime affects on text'
description: I extended the TextMeshPro Unity package to include three new tags, which have runtime affects on text
---

Recently, I've been thinking about starting to create tools and packages for Unity and decided a good place to start would be adding some functionality to an already existing package - TextMeshPro(TMP)! Something that I have seen around is text effects that change over time. Wavy text, shaky text, text which changes colour. TMP even has examples of these but they apply themselves to all the text in a component. Other people I have found have implemented them in ways which allow them to tag and have effects only appear on certain letters, but these implementations parse the text themselves and write their own tag recognition. In my extension to the package I decided to dig deeper and instead add the tags to the pre-existing TMP tags and utilise its tag parsing myself. I then added the functions to apply the effects and the settings which I wanted for the effects to the TMP components that already exist. This all means that the custom TMP package works out the box, tagging in the same way as other TMP tags and without having to add another component/script to any game objects that you want to have them.

See the repository: [github.com/twood27897/customtextmeshpro](https://github.com/twood27897/customtextmeshpro)<br/>
