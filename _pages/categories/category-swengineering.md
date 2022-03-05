---
title: "Software Engineering"
permalink: /categories/software engineering/
layout: category
author_profile: true
sidebar_main: true
---

Software Engineering

{% assign posts = site.categories['software engineering'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}