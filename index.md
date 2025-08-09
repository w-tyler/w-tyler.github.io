---
layout: default
title: 首页
---

# 欢迎来到我的博客

这是基于Jekyll和GitHub Pages搭建的个人博客。在这里我会分享技术文章、生活感悟和学习心得。

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
      • {{ post.categories | join: ", " }}
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

## 关于博客

这个博客使用Jekyll静态网站生成器构建，托管在GitHub Pages上。支持Markdown写作，具有以下特性：

- 📝 支持Markdown语法
- 🏷️ 文章分类和标签
- 📱 响应式设计
- 🔍 SEO友好
- 📡 RSS订阅
- 💬 评论系统（可配置）

开始写作吧！在 `_posts` 目录下创建新的Markdown文件即可发布文章。 