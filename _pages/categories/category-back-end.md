---
title: "Back-end Categories"
permalink: /categories/back-end/
layout: category
author_profile: true
sidebar_main: true
---

Back-end contents

{% assign posts = site.categories.back-end %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}