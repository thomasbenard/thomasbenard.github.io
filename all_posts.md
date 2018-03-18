---
title: Post History
layout: default
---

# Post History

{% for post in site.posts %}  
__[{{post.title}}]({{post.url}})__  
_{{post.date | date: "%-d %B %Y"}}_  
{{post.excerpt}}
 {% endfor %}

