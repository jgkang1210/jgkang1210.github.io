---
layout: page
permalink: /publications/
title: publications
description: publications
years: []
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% raw %} {% bibliography -f papers -q @*[year={{y}}]* %}{% endraw %} 
{% endfor %}

</div>
