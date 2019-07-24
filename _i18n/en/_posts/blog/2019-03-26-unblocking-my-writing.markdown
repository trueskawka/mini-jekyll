---
layout: post_default
title:  "Unblocking my writing"
date:   2019-03-26 20:12:00 +4:00:00
categories: ["blog"]
author: "alicja"
published: true
---

I often self-identify as a person who reads and writes a lot. Yet it has been challenging to write for the past few months. I wanted to explore why, as my actions were not aligned with my goals.

I realized I have blocked myself. Yes, writing is hard, especially writing and sharing, but that has not stopped me before. It was the workflow I dreaded that was blocking me.

### When your blog build takes 140 seconds
I have used Jekyll for my previous website. I kept using it for nearly 3 years without any refactoring or reorganizing the content. The site grew heavy with posts from different contexts, translations, buildup of old projects, abandoned ideas, drafts, and what-ifs. I wrote a few clunky Ruby plugins to use in specific contexts. I was not finding them useful anymore, but I did find it hard to part with them.

The legacy code and content caused the build time to slowly creep up from a few seconds to well over two minutes. Each time I saw a typo or wanted to play around with content, I could either do it directly in developer tools or wait for the files to rebuild. Not even `--watch` helped here much, as I bloated my build script with unnecessary workload.

This made it incredibly difficult to work with, as I was unable to see my changes quickly and iterate on them. Every blog post became a painful process.

I only mustered to go through it once every few days. Then every few weeks. Finally even a few months was not enough time to push myself to work with it.

### I just need a blog
I like the idea of using the simplest possible tool for the job. I have a website mostly to share blog posts and update my resume from time to time. I misattributed my Jekyll issues to Jekyll, without looking too closely at the thousands of lines of legacy code hidden neatly in numerous directories, and I was reluctant to try it again.

Once every few months I'd try a new shiny idea for creating a workflow - Hakyll, Gatsby, WordPress. I even considered hosting Jupyter notebooks as I found it easier to work in them and review my work in chunks. But each of those ideas caused more problems than it offered solutions, as I wanted my website to be simple, static, and fast.

I revisited Jekyll, but instead of trying to refactor, I started with a clean slate. I took a few days to create a design without any unnecessary assets (turns out pretty much all of them were unnecessary). I coded the blog in a few hours. Migrated the posts I cared about and installed two plugins I needed. Then put back everything else I wanted to preserve as static assets, as to not bloat my build process.

It takes 3.5 seconds now.

### Those Jupyter notebooks sound pretty neat tho
For some reason, Jupyter notebooks still sounded appealing. It turns out the CSS blog posts I've been painstakingly managing with huge CSS files and handcrafted code chunks in Markdown can be created without much friction using Jupyter `display` built-ins.

Lo and behold, I can iterate on my ideas and write CSS notes, saving each step of the way while exploring (more on that soon). No more "fork CodePen every time something fun happens". No more "I just need to render this thing in a browser". Open a notebook, explore, and export to Markdown.

### If you find a workflow painful, there might be something to it
I was writing off issues with my workflow as laziness and lack of creativity. I was _obviously_ not determined enough to write - otherwise all the issues with long build times, slow website and handcrafted CSS would not stop me. If I was a real blogger...

Turns out I'm still a blogger at heart and my discomfort was valid. Now I can't wait to hack away on CSS and generate ideas that I'll be able to quickly share. Granted, I have not figured out a way to work on fewer ideas at the same time yet. Hopefully it's a sign of excitement.