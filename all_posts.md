---
layout: default
---

# Post History

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}"><strong>{{ post.title }}</strong></a>
      <br/><em>{{ post.date | date: "%-d %B %Y" }}</em>
      <br/>{{ post.excerpt }}
    </li>
  {% endfor %}
</ul>

