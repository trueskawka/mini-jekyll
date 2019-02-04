---
layout: post_default
title:  "Outreachy - first month recap"
date:   2017-01-11 23:48:00 +4:00:00
categories: ["zulip", "outreachy", "gci", "blog"]
author: "alicja"
published: true
---
I've been asked today to reflect on my first Outreachy month and share
what I've done so far.

### Initial proposal

I admit I've felt queasy about it at first - after
all, my initial [proposal](
  https://paper.dropbox.com/doc/Outreachy-proposal-QfwrTnnGBDBd9hBGCyUjT)
was full of ambitious goals that were to be driven by the regular expected
levels of productivity.

That didn't happen. I have made mistakes on the way:

* I let my momentum get lost while visiting family for the better part of
  November and it took time to regain it after I got back home.

* I am still struggling with sharing imperfect code and contributions, even
  though as a mentor I know their value.

* I got completely involved in the Google Code-In, because after the initial
  stress of being inexperienced and unable to help the students, it became
  easier and easier with every week to focus on it.

* I got used to my to-do list getting longer and longer, focusing more on
  immediate tasks than long-term goals. It's easier to tick off "create
  a GCI task" than "implement this big and fuzzy issue".

What it meant for my proposal - I haven't worked on most of my planned tasks
when planned:

* The CSS splitting got behind the Zulip redesign and I was able to split
  only one part since my pre-Outreachy splits, as everything else is in the
  midst of changes and refactoring.

* Building screenshots and improving Casper tests is still on my to-do list,
  but the only steps I've taken so far is reading the Casper docs, tinkering
  a bit with our Casper doc and refactoring the task into more actionable
  steps earlier this week.

* My work on interactive bots was late, but since it was part of my GCI
  responsibilities, I've managed to learn how to create bots and wrote the
  tasks for them. Over the past few weeks I've also helped students work
  on the bots and learned a lot from them (more on that later on).

* I haven't worked on issues related to stream flow.

### What I have been working on

My contributions concerned mostly GCI improvements and communication, with
some minor work towards my general goals. I've found it much easier to
write and be social - respond to messages, review pull requests, help
students with tasks, create documents - than to focus on my own code.

* I have created the [interactive bots tasks](
  https://github.com/zulip/zulip-gci/blob/master/tasks/interactive-bots.md)
  for the GCI students.

* I have created the [translation tasks](
  https://github.com/zulip/zulip-gci/blob/master/tasks/translations.md) for
  the GCI students and wrote the [initial Polish style guide](
    https://github.com/zulip/zulip/pull/2439).

* I have written a doc on [improving the GCI experience](
  https://github.com/zulip/zulip-gci/blob/master/docs/improving-gci-experience.md)
  as a reflection and a shared set of experiences. I have also written an
  internal doc for mentors on encouraging students.

* I [split the right sidebar CSS](https://github.com/zulip/zulip/pull/2128)
  from the main `zulip.css` file.

* I have had 3 GCI shifts a week for reviewing student submissions and
  helping them on Zulip. I've also been helping students, posting various
  reflections, and discussing issues and improvements quite a lot.

I also currently have multiple tasks in line:

* I posted a work-in-progress pull request on a [muted streams prompt](
  https://github.com/zulip/zulip/pull/2721) that needs refactoring and
  better testing.

* I'm working on building CSS linters after a [fruitful chat with Steve](
  https://chat.zulip.org/#narrow/stream/test.20suites/topic/linting) [you
  have to be logged in [Zulip](https://chat.zulip.org)]. I am also creating
  a write-up of this discussion for further use.

* I have a React Native app [issue assigned](
  https://github.com/zulip/zulip-mobile/issues/181) - so far I've a working
  environment and I've tried to trigger the issue with no luck on my mobile
  device.

* I have a draft write-up about Casper testing and will be improving the
  existing [Zulip Casper doc](
    https://zulip.readthedocs.io/en/latest/testing-with-casper.html).

* I have a few draft posts on mypy, open-source skills and improving as an
  intermediate programmer.

### Actionable goals

For the next two months of my internship, I want to focus more on my own
contributions, especially putting my coding skills to good use. Google
Code-In is ending soon, which will free up my schedule and prevent me from
focusing entirely on mentoring. Alas, I need some actionable goals, since
my initial proposal was too vague and I got easily distracted with better
defined tasks.

* I will refactor each of the tasks currently in line (and any future task)
  and split them into smaller, better defined ones, with more specific
  deadlines.

* I will check in daily, as it's much harder to be vague when informing
  others what I've been up to.

* I will set aside time for big picture ideas on possible enhancements and
  creative writing on non-technical aspects of software development and I
  will be focused on the task at hand outside of those intervals.

* I will follow a more regular schedule instead of working in irregular
  intervals of varied lengths.
