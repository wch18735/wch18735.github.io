---
title: "Git Categories"
permalink: /categories/git/
layout: category
author_profile: true
sidebar_main: true
classes: wide
---

Python code segments and tips

{% assign posts = site.categories['git'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}