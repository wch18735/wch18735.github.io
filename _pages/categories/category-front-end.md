---
title: "Front-end Categories"
permalink: /categories/front-end/
layout: category
author_profile: true
sidebar_main: true
---

Front-end projects

{% assign posts = site.categories.front-end %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}