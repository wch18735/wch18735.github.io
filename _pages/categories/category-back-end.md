---
title: "Back-end Categories"
permalink: /categories/back-end/
layout: category
author_profile: true
sidebar_main: true
---

This category contatins backend development contents for :

- JSP
    - GET, POST
    - Tomcat
    - Servlet

{% assign posts = site.categories.back-end %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}