---
title: "Legacy Survival"
permalink: /categories/legacy survival/
layout: category
author_profile: true
sidebar_main: true
---

레거시에서 살아남기. 레거시를 유지/보수하며 겪은 일들을 하나씩 기록한다.

이렇게 쌓인 기록들이 언젠가 도움이 되는 날이 오겠지.

{% assign posts = site.categories['legacy survival'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}