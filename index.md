---
layout: default
title: 首页
---

# 🌳 欢迎来到天一的树洞

这里是一个温馨的小角落，就像一棵大树下的树洞，安静而充满生机。在这里，我会轻声分享生活中的点点滴滴、技术路上的小小收获，以及那些在某个瞬间触动心弦的感悟。

每个人都需要一个属于自己的小天地，而这里就是我的。也希望路过的你，能在这里找到一丝温暖，一点共鸣。

## 最新文章

<div class="posts">
{% for post in site.posts limit:5 %}
  <article class="post-preview">
    <h2>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </h2>
    <p class="post-meta">
      {{ post.date | date: "%Y年%m月%d日" }}
      {% if post.categories.size > 0 %}
      • {% for category in post.categories %}
        <a href="{{ '/categories' | relative_url }}#category-{{ category | slugify }}" class="category">{{ category }}</a>{% unless forloop.last %}, {% endunless %}
      {% endfor %}
      {% endif %}
    </p>
    <div class="post-excerpt">
      {{ post.excerpt }}
      <a href="{{ post.url | relative_url }}">阅读全文 →</a>
    </div>
  </article>
{% endfor %}
</div>

{% if site.posts.size > 5 %}
<p><a href="{{ '/archive' | relative_url }}">查看所有文章 →</a></p>
{% endif %}

## 🌱 关于这个树洞

这里就像是一个神奇的树洞，能够承载文字的力量，让思绪在这里生根发芽。无论是：

- 💭 日常的小思考
- 🔧 技术学习的笔记
- 📚 读书时的心得体会
- 🌸 生活中的美好瞬间
- 🎨 创作的灵感火花
- 🤝 与朋友们的交流分享

都可以在这里安静地绽放。

每一篇文字都是时间的礼物，每一次分享都是心灵的对话。愿这个小小的树洞，能成为连接彼此内心的桥梁。

*轻轻地来，轻轻地分享，在文字的森林里漫步。* 🌿 