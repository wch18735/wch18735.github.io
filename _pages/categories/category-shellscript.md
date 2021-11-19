---
title: "Shell Script"
permalink: /categories/shellscript/
layout: category
author_profile: true
sidebar_main: true
---

Shell Script Category

{% assign posts = site.categories['shellscript'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}