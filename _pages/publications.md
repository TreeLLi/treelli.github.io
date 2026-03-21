---
layout: page
permalink: /publications/
title: Publications
description:
years: [2026,2025,2024,2023]
nav: true
nav_order: 1
---
\* indicates equal contribution; † indicates the corresponding author if have.
A latest list of publications can be found at [Google scholar](https://scholar.google.com/citations?user=dxP6Y_oAAAAJ&hl=en).

<!-- _pages/publications.md -->

<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* -T bib2 %}
{% endfor %}

</div>
