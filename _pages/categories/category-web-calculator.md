---
title: "Todo Application"
permalink: /categories/web calculator/
layout: category
author_profile: true
sidebar_main: true
---

Web calculator API

{% assign posts = site.categories['web calculator'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}