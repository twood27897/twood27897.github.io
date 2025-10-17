---
layout: post
title: 'SELA (Software Engineers Love Acronyms)'
author: Thomas Wood
tagline: 'SELA (Software Engineers Love Acronyms)'
description: SELA (Software Engineers Love Acronyms)
---

<p align="left">
  After writing my <a href="https://helloimtw.me/pages/blogs/applyingsolidprinciples.html">Applying SOLID Design Principles</a> blog last week I got thinking about just how many acronyms I've come across since entering the world of professional software engineering. Acronyms (or backronyms or abbreviations or however else you want to  categorise them) get thrown around a fair amount in software circles. Although they are used with the noble intention of making us better at programming, they are a double edged swored, useful but also, at times, misleading. I thought to help myself dig a little deeper into a couple of them I would write something. What follows isn't an exhaustive list by any means, just a quick summary of my subjective view on a select few popular acronyms that I have found in my time as a programmer.<br/><br/>

<b>DRY - Don't Repeat Yourself</b><br/><br/>

Simple right? Write DRY code! If you find yourself copying and pasting code, ask yourself if it could be turned into a function or class or abstraction of some kind. This can help simplify and save a lot of time in the future, as it means there is only one point of truth for the functionality that now exists behind the name of a function/class/etc. When that functionality needs edited or changed you no longer need to try and remember all the other places the copy pasted code exists and if anyone else needs that functionality they can use the function/class/etc you have made. But wait! As with all these principles and best practices, we must use them in moderation and not push them too far. Dan Abramov has a great <a href="https://www.deconstructconf.com/2019/dan-abramov-the-wet-codebase">talk</a> that shows how this principle can lead to badness. I find a good rule of thumb is if you're repeating yourself within a class/function you should almost definitely clean it up (make it DRY!) but if you're repeating yourself across multiple systems/classes you should slow down, examine the problem more closely and make sure you don't rush into creating a complex dependency/abstraction headache.<br/><br/>

<b>YAGNI - You Aren't Gonna Need It</b><br/><br/>

YAGNI is all about not over planning and over complicating your code before you need to. Don't add functionality until it is deemed necessary! I have experienced first hand how extra effort put into creating a piece of software can end up wasted as design needs change. What originally seems like a no brainer or a pre-emptive win or bit of foresight can easily become lost time. If only YAGNI had been considered! That being said, underplanning is never the right answer either. You have to strike a balance. If YAGNI becomes an excuse for over simplification that results in code that is hard to extend or creates mess, then something has gone wrong. Also it is worth noting that originally YAGNI was meant to be considered as a part of the Extreme Programming methodology, alongside practices like continuous refactoring and automated testing. If used outside of that context, it should be used with caution.<br/><br/>   

<b>KISS - Keep It Simple Stupid</b><br/><br/>

Inversely, don't make things too complex cleverclogs! KISS itself seems simple at first but when applied in a software setting it can get out of hand and begin to raise questions. Keep what simple? How can I program to solve complex problems while keeping it simple? Isn't simplicity a subjective concept anyway? I don't really think there is a right answer here. Personally, I think I like KISS most when applied to the readability of individual lines of code. Programming languages are filled with syntax and idioms that can be combined in ways that could confuse the reader. For that reason, if there is a simple way to write a line of code that doesn't require a junior engineer's head to explode upon reading, then KISS!<br/><br/>

<b>TMA - Too Many Acronyms</b><br/><br/>

I first saw this one as a comment in response to a piece of documentation that contained a number of acronyms without their relevant explanations. The author had to ask what TMA stood for, perfectly exemplifying the problem they had created for the reader. It is easy to get into a habit of throwing around acronyms that you perceive to be well know. I'm sure I have been guilty of this - I've written a blog about them after all! But what better way to help the acronym lover than to give them a new acronym? TMA is a great check. While all the acronyms I've talked about have real value, they should never be overused in conversation with the assumption that everyone knows what they mean. I find them more useful as internal reminders and jumping off points for debate rather than as quick answers or easy comebacks. Except maybe in the case of TMA.<br/><br/>

<b>NARUTO - Never Apply Rules Unquestioningly; Think & Observe</b><br/><br/>

Probably the most relevant acronym for this blog, I am not actually sure if it exists outside of <a href="https://www.reddit.com/r/programming/comments/1bmicj0/3_software_development_principles_i_wish_i_knew/">this</a> reddit thread. A great way to summarise how you should approach applying almost any set of rules, <i>especially</i> the ones in this blog. Always understand what you are doing and why you are doing it, don't overzealously apply and reapply any rule. Think, observe, question. But also probably apply NARUTO to NARUTO! Don't think, observe and question too much!   
</p>
