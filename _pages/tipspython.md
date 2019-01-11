---
layout: archive
title: "Big Data"
permalink: /bigdata/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.bigdata reversed %}
  {% include archive-single.html %}
{% endfor %}
