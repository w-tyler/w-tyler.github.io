---
layout: post
title: "欢迎使用Jekyll！"
date: 2024-01-15 10:00:00 +0800
categories: [Jekyll, 博客]
tags: [Jekyll, GitHub Pages, 静态网站]
author: Tyler
---

欢迎来到Jekyll！这是你的第一篇博客文章。

## 什么是Jekyll？

Jekyll是一个简单的、适合博客的静态网站生成器。它可以将纯文本转换为静态网站和博客。Jekyll是GitHub Pages的引擎，这意味着你可以免费在GitHub上托管你的Jekyll博客。

## 主要特性

Jekyll具有以下特性：

- **简单**: 无需数据库，无需评论审核，无需更新 - 只需要你的内容
- **静态**: Markdown、Liquid模板和HTML & CSS构建静态网站
- **免费**: 在GitHub Pages上免费托管
- **支持博客**: 支持永久链接、分类、页面、文章以及自定义布局

## 如何写文章

要发布新文章，只需在 `_posts` 目录下创建新文件即可。文件名格式必须遵循：

```
YEAR-MONTH-DAY-title.md
```

例如：
```
2024-01-15-welcome-to-jekyll.md
```

## Front Matter

每篇文章都需要在开头添加YAML格式的Front Matter：

```yaml
---
layout: post
title: "文章标题"
date: 2024-01-15 10:00:00 +0800
categories: [分类1, 分类2]
tags: [标签1, 标签2, 标签3]
author: 作者名
---
```

## Markdown语法

Jekyll支持完整的Markdown语法。你可以使用：

### 标题
```markdown
# 一级标题
## 二级标题
### 三级标题
```

### 强调
```markdown
**粗体文本**
*斜体文本*
***粗斜体文本***
```

### 链接和图片
```markdown
[链接文字](http://example.com)
![图片描述](path/to/image.jpg)
```

### 代码
```markdown
行内代码：`code`

代码块：
```python
def hello_world():
    print("Hello, World!")
```

### 列表
```markdown
- 无序列表项1
- 无序列表项2

1. 有序列表项1
2. 有序列表项2
```

## 开始写作吧！

现在你已经了解了Jekyll的基本用法，可以开始创建你自己的博客文章了。只需在`_posts`目录下创建新的Markdown文件，Jekyll就会自动处理并生成静态页面。

祝你写作愉快！🎉 