---
title: "Calculus"
layout: archive
permalink: categories/calculus
author_profile: true
types: posts
---

{% assign posts = site.categories['calculus']%}
{% for post in posts %}
{% include archive-single.html type=page.entries_layout %}
{% endfor %}