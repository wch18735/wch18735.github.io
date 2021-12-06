---
title: "Spring"
permalink: /categories/spring/
layout: category
author_profile: true
sidebar_main: true
pagination:
    enabled: true
    category: spring
---

{% assign posts = site.categories.spring %}
{% for post in posts %} 
    {% include archive-single.html type=page.entries_layout %} 
{% endfor %}