---
layout: post_default
title:  "Browsing in grayscale"
date:   2018-07-20 12:50:00 +4:00:00
categories: ["blog"]
author: "alicja"
published: true
---
I started exploring my browsing habits to better understand why certain websites grab my attention.
As aware of UX dark patterns as I am, I grew tired of mindfully noticing when they want to "nudge" 
me into certain behaviors with vibrant colors and obscuring others by making them less visually
appealing.

While I was trying to unsubscribe from yet another unwanted email marketing list, chasing for the 
"unsubscribe" link hidden in plain sight, I noticed that grayscale works well for techincally showing the 
information required of advertisers, yet obscuring it among the flashy colorful action calls. 

My previous attempt at decreasing the time I spend on social media was leveraging [Stylus](https://addons.mozilla.org/en-US/firefox/addon/styl-us/)
to hide certain parts of websites, e.g. related videos and comments on YouTube. It manages user styles, 
with a simple checkbox menu for enabling and disabling them.

Since I use FireFox, switching to grayscale is fairly simple. I created a new stylesheet that applies to 
all websites and added a single property for `body`. (you can also add a filter in [other browsers](https://superuser.com/questions/330830/how-to-turn-web-pages-in-browser-into-black-white-grayscale))

```
body {
  filter: grayscale(100%);
}
```

It's easy to turn it off when I want to. And it encouraged me to fix minor annoyances on some websites. 
I stopped using AdBlock when I switched to FireFox and I now use targeted `display: none` on elements 
I find distracting.

A minor caveat of the extension is that it doesn't work on all websites, because it's a WebExtension. 
I found that it only makes me appreciate it more when I encouter a page that doesn't apply my user 
styles. 

One more surprising issue I encountered is that the `grayscale` filter sometimes breaks scrolling. I tried 
debugging it on my website, thinking it might be caused by some legacy code or a hidden Stylus property, 
but adding it directly to the `body` also changed scrolling behavior.