---
title: "Deep Learning"
permalink: /categories/deep learning/
layout: category
author_profile: true
sidebar_main: true
---

무겁지 않게 쓰는 Deep Learning

{% assign posts = site.categories['deep learning'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}