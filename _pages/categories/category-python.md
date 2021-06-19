---
title: "Algorithm Categories"
permalink: /categories/python/
layout: category
author_profile: true
sidebar_main: true
classes: wide
---

Python code segments and tips

{% assign posts = site.categories['python'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}