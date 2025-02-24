---
layout: page
title: About
permalink: /about/
---

{% for section in site.data.about %}

## {{ section.section }}

{{ section.text | markdownify }}
{% endfor %}
