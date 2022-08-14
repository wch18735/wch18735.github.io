---
title: "Numpy"
permalink: /categories/numpy/
layout: category
author_profile: true
sidebar_main: true
pagination:
    enabled: true
    category: numpy
---

{% assign posts = site.categories['numpy'] %}
{% for post in posts %} 
    {% include archive-single.html type=page.entries_layout %} 
{% endfor %}