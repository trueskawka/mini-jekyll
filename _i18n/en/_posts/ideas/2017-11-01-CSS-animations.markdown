---
title:  "Drawing with CSS"
technology: CSS
categories: ["idea"]
author: "alicja"
link: "https://github.com/trueskawka/css-drawings"
---

I wanted to learn better CSS and have some fun while exploring it,
so I started drawing logos and simple images using pure CSS.

Check out my tutorials:
{% for post in site.posts %}
{% if post.categories contains 'css' %}
- <a href="{{ post.link }}">{{ post.title }}</a>
{% endif %}
{% endfor %}

