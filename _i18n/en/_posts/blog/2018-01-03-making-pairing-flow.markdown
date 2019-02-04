---
layout: post_default
title:  "Making pairing flow"
date:   2018-01-03 17:25:00 +4:00:00
categories: ["blog", "programming"]
author: "alicja"
published: true
---
As my second batch at the Recurse Center is approaching, I can't help
but to be excited for all the pairings to come. I enjoy learning by 
working with others - other than improving my coding style and understanding
how to solve problems better, I always pick up all sorts of little tweaks 
and ideas while geeking out over someone's setup.

Since many of my batchlings are going to be new to the community, I wanted to think
of some rules for pairing that could improve the experience for all those involved,
other than the [Recurse Center social rules](https://www.recurse.com/manual#sub-sec-social-rules).

In general, I find there are a at least three types of potentially great pairing 
experiences:
- helping someone out - when one of the parties has an issue and they're looking 
for some guidance from a pairing partner more experienced in the area of interest
- exploring a concept together - when both parties are looking forward to tackle
a new problem together, regardless of their perceived experience
- rubber-ducking - when one of the parties needs to talk to another programmer, 
be it on a more general, problem-solving level or particulars of their code that
make for a mind-boggling puzzle they're struggling with, again - regardless of their 
perceived experience (it is often the "stupid" or "beginner" questions that we 
forget to ask ourselves when debugging our own code that are just the right questions)

### Have a good setup

Don't assume your setup is going to work for pairing out of the box. Think of possible 
obstacles and make sure your pairing partner is comfortable, e.g. I promise to 
increase my 8-point font and adjust the brightness, so you can read everything you 
need on my tiny screen.

The best approach is to find the lowest common denominator - no Vim/Emacs bindings, 
no dark magic shortcuts and gear. Using a text editor with at least some GUI (VS Code, 
Atom), a separate window for the terminal and a browser window for web testing usually 
works. Sometimes even a simple service like [Glitch](https://glitch.com/) or [CodePen](https://codepen.io/) 
is enough.

### Keyboard is lava

During pairing it's usually best that only one person types - they're called the 
driver and their role is to implement your shared ideas in code. The more beginner 
they are, the better for them to be the person writing the code, since they can 
make mistakes and learn from them through discussing and correcting them together. 
They won't learn as much from simply observing.

Even if you are much better at typing, please, don't take over the keyboard. It 
happened to me more than once, usually out of the sheer eagerness of my pairing 
partner to just solve the problem at hand, and I always felt intimidated by it.

### Know what's important

When pairing, try to focus on the problem at hand - you are giving/receiving the most
important asset one can give, their time and attention. There is no point in focusing
on unrelevant details, e.g. I might write terribly suboptimal Python, but if the 
purpose of the pairing is first and foremost for me to understand how merge sort works,
optimizing too early might not be all too helpful. 

1. When helping - depending of how experienced your pairing partner is, there might be 
    an urge to solve every possible problem together - if you have a ton of time, that's a 
    wonderful way to spend the afternoon/night! Learning from more experienced developers
    is always fun, especially when they can also learn a trick or two from their 
    pairing buddies.

    Most of the time the pairing is focused around a particular problem. If you are 
    helping someone out, they probably went through at least a few ideas and have tried
    various approaches. Before you get started, make sure you know:
    - what is the problem
    - what has already been tried
    - what kind of help are they looking for

2. When asking for help - [before you ask, try](https://blogs.akamai.com/2013/10/you-must-try-and-then-you-must-ask.html). 
    Try to understand where and why you have a problem, so that you can describe it to 
    your pairing partner. Check for all the usual suspects (semicolons, not passing 
    arguments, not calling the function, typos) - you shouldn't be embarassed if your 
    problem turns out to be "just" about a typo, but more often than not you probably will 
    feel bad for "wasting" someone's time (please, don't - you learn by making mistakes).

    Before you get started:
    - give a brief overview of your code
    - describe the problem and the approaches you already tried
    - talk about your hypotheses as to where the issue might be
    - say what kind of help you are looking for

3. When pairing on a new problem  - be it a "Cracking the Coding Interview" 
    question or a project you are building together, try to define what are you looking to 
    do during the pairing session. Decide together on the setup, stack and other relevant
    technical details. 

### Be mindful

Pairing is meant to be a pleasurable experience for all parties involved, but we do 
make mistakes - you might tell an unfortunate joke or the solution you propose ends 
up causing a stack overflow. That's all good, as long as you are mindful and open 
to correcting any missteps.

Think of pairing as canoeing together (or dancing, if that's a more appealing 
metaphor). There is a rhythm to it and a direction you're working towards, but there
is also the pleasure of being in the moment together and enjoying the experience. If 
you're going to go in different directions or not find a shared rhythm to it, the canoe
might end up spinning in the middle of the river and you will get nowhere.

Finding the right balance might take time, but it's rewarding. Also, you don't have 
to have a great experience with everyone - even when you're both excited about 
programming and mindful, you might just not be good pairing partners. And that's okay.
