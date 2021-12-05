---
title: "JSP Categories"
permalink: /categories/jsp/
layout: category
author_profile: true
sidebar_main: true
pagination:
    enabled: true
    category: jsp
---

This category contatins JSP development contents for :

- JSP
    - GET, POST
    - Tomcat
    - Servlet

{% assign posts = site.categories.jsp %}
{% for post in posts %} 
    {% include archive-single.html type=page.entries_layout %} 
{% endfor %}