---
title: "Kubernetes"
permalink: /categories/kubernetes/
layout: category
author_profile: true
sidebar_main: true
---

Kubernetes Category

{% assign posts = site.categories['kubernetes'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}