---
title: First attempt at new year resolutions
toc: true
toc_label: "Goals"
---

Hey everyone, welcome to my site which will serve to document my goals, projects, thoughts and whatever else might happen to need to get documented.

I've never really done new years resolutions before and decided on a whim (after watching some cool Youtube video about how people automate their daily routines) that I should start making some changes... by which I mean track what I do and figure out my stats (and eventually set goals for myself).

This first post is to document what I plan to work on and what the goals for 2018Q1 are.

I'll be cleaning this up (in the [code](https://github.com/MarvinT/MarvinT.github.io) as I learn Jekyll, markdown, liquid, yaml, HTML, and whatever else) and updating this page as I make progress.

{% assign sorted_goals = (site.goals | sort: 'order') %}

<table>
    <thead>
      <th>Goal</th>
      <th>Description</th>
    </thead>
    <tbody>
     {% for goal in sorted_goals %}
     <tr>
        <td><a href="{{ goal.title | slugify | prepend: "#" }}">{{ goal.title }}</a></td>
        <td> {{ goal.description }} </td>
     </tr>
     {% endfor %}
    </tbody>
</table>

{% for goal in sorted_goals %}
  {{ goal.title | prepend: "## " | markdownify }}
  <p>Description: {{ goal.description }}</p>
  <p>Trackable metrics: {{ goal.metrics }}</p>
  <p>Approaches: {{ goal.approaches }}</p>
  <p>Q1 explicit goals: {{ goal.g2018Q1 }}</p>
  <p>2018 goals: {{ goal.g2018 }}</p>
  {% if goal.links %}
  <p>Relevent Links:</p>
  <ul>
  {% for link in goal.links %}
  <li><a href="{{ link.url }}">{{ link.text }}</a></li>
  {% endfor %}
  </ul>
  {% endif %}
{% endfor %}
