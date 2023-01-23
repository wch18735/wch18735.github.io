---
title: "Starbucks project"
permalink: /categories/starbucks/
layout: category
author_profile: true
sidebar_main: true
---

Starbucks mocking project with html, css, javascript and typescript

{% assign posts = site.categories['starbucks'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}