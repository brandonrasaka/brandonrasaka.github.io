---
layout: default
title: Projects
nav_order: 2
---

## Here are some of the projects I've worked on

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>