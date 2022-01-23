---
title: "Java"
permalink: /categories/java/
layout: category
author_profile: true
sidebar_main: true
pagination:
    enabled: true
    category: java
---

{% assign posts = site.categories. %}
{% for post in posts %} 
    {% include archive-single.html type=page.entries_layout %} 
{% endfor %}