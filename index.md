---
layout:	default
title: The Way of the Craftsman
---

# Last Posts

<ul>
  {% for post in site.posts limit:10 %}
    <li>
      <a href="{{ post.url }}"><strong>{{ post.title }}</strong></a>
      <br/><em>{{ post.date | date: "%-d %B %Y" }}</em>
      <br/>{{ post.excerpt }}
    </li>
  {% endfor %}
</ul>

