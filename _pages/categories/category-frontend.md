---
title: "Javascript Categories"
permalink: /categories/frontend/
layout: category
author_profile: true
sidebar_main: true
---

These projects are mainly based on javascript. Furthermore, These posts are including sort of html, css contents too.

{% assign posts = site.categories['frontend'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}