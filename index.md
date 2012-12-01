---
layout: page
title: "it-spir.it - Python, Zope & Plone Development"
---
{% include JB/setup %}

## Recent Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

<a href="https://github.com/tmassman" id="view-on-github" class="btn btn-success"><span>View on GitHub</span></a>
