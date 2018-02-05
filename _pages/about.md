---
permalink: /about/
title: "About Marvin Thielk"
ads: false
share: false
---

{% assign sorted_figures = (site.figures | sort: 'order') %}

{% include my_gallery caption="Attempts to quantify who I am..." %}

{% for my_figure in sorted_figures %}
  {% include my_figure.html %}
{% endfor %}
