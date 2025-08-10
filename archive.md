---
layout: default
title: 文章归档
permalink: /archive/
---

# 文章归档

<div class="archive">
{% assign postsByYear = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}
{% for year in postsByYear %}
  <h2>{{ year.name }}</h2>
  <ul class="archive-list">
  {% for post in year.items %}
    <li>
      <span class="archive-date">{{ post.date | date: "%m-%d" }}</span>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.categories.size > 0 %}
        <span class="archive-categories">
          {% for category in post.categories %}
            <a href="{{ '/categories' | relative_url }}#category-{{ category | slugify }}" class="category">{{ category }}</a>
          {% endfor %}
        </span>
      {% endif %}
    </li>
  {% endfor %}
  </ul>
{% endfor %}
</div>

{% if site.posts.size == 0 %}
<p>还没有发布任何文章。<a href="{{ '/' | relative_url }}">返回首页</a></p>
{% endif %} 