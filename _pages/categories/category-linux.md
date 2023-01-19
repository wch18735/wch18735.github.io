---
title: "Linux"
permalink: /categories/linux/
layout: category
author_profile: true
sidebar_main: true
---

Linux related category

{% assign posts = site.categories['linux'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}