---
title: "Statistics"
layout: "archive"
permalink: "/categories/statistics"
author-profile: true
sidebar:
    nav: "sidebar-category"
---
통계학 정리노트

{% assign posts = site.categories['algorithm'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
