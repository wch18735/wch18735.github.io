---
title: "JPA Category"
permalink: /categories/jpa/
layout: category
author_profile: true
sidebar_main: true
---

JPA cateogry.

{% assign posts = site.categories['jpa'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}