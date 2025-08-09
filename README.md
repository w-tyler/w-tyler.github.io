# Jekyll 博客

这是一个基于Jekyll和GitHub Pages搭建的个人博客。

## 快速开始

### 在线访问

博客已部署到GitHub Pages，访问地址：https://w-tyler.github.io

### 本地开发

1. 克隆仓库：
```bash
git clone https://github.com/w-tyler/w-tyler.github.io.git
cd w-tyler.github.io
```

2. 安装依赖：
```bash
bundle install
```

3. 启动本地服务器：
```bash
bundle exec jekyll serve
```

4. 在浏览器中访问 `http://localhost:4000`

## 写作指南

### 创建新文章

在 `_posts` 目录下创建新的Markdown文件，文件名格式：

```
YYYY-MM-DD-title.md
```

例如：`2024-01-15-my-first-post.md`

### Front Matter

每篇文章都需要包含YAML Front Matter：

```yaml
---
layout: post
title: "文章标题"
date: 2024-01-15 10:00:00 +0800
categories: [分类1, 分类2]
tags: [标签1, 标签2]
author: 作者名
---
```

### Markdown语法

Jekyll支持标准的Markdown语法，包括：

- 标题 (`#`, `##`, `###`)
- 强调 (`**粗体**`, `*斜体*`)
- 链接 (`[文字](URL)`)
- 图片 (`![描述](URL)`)
- 代码块 (`` ` `` 和 ``` ```)
- 列表 (`-` 和 `1.`)

## 目录结构

```
.
├── _config.yml          # Jekyll配置文件
├── _layouts/            # 页面布局模板
│   ├── default.html     # 默认布局
│   └── post.html        # 文章布局
├── _posts/              # 博客文章
├── assets/              # 静态资源
│   └── css/             # 样式文件
├── about.md             # 关于页面
├── archive.md           # 文章归档
├── index.md             # 首页
├── Gemfile              # Ruby依赖
└── README.md            # 项目说明
```

## 部署

推送到GitHub后，GitHub Pages会自动构建和部署网站。确保在仓库设置中启用了GitHub Pages。

## 自定义

- 修改 `_config.yml` 来更改网站配置
- 编辑 `assets/css/style.scss` 来自定义样式
- 在 `_layouts` 目录下修改或添加新的布局模板

## 功能特性

- ✅ 响应式设计
- ✅ 文章分类和标签
- ✅ 文章归档
- ✅ 代码高亮
- ✅ SEO优化
- ✅ RSS订阅
- ✅ 社交链接

## 更多资源

- [Jekyll官方文档](https://jekyllrb.com/docs/)
- [GitHub Pages文档](https://docs.github.com/en/pages)
- [Markdown语法指南](https://www.markdownguide.org/)

祝你写作愉快！ 🎉 