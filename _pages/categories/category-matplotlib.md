---
title: "Matplotlib"
permalink: /categories/matplotlib/
layout: category
author_profile: true
sidebar_main: true
pagination:
    enabled: true
    category: matplotlib
---

{% assign posts = site.categories['matplotlib'] %}
{% for post in posts %} 
    {% include archive-single.html type=page.entries_layout %} 
{% endfor %}