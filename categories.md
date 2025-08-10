---
layout: default
title: 分类
permalink: /categories/
---

<div class="category-header">
  <h1>📂 文章分类</h1>
  <p>探索不同主题的精彩内容，找到你感兴趣的文章</p>
</div>

<div class="category-grid">
  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
    <div class="category-card">
      <h3>{{ category[0] }}</h3>
      <p>{{ category[1].size }} 篇文章</p>
      <span class="category-count">{{ category[1].size }}</span>
      <div class="category-preview">
        {% for post in category[1] limit:3 %}
          <small>• {{ post.title }}</small><br>
        {% endfor %}
        {% if category[1].size > 3 %}
          <small>• ...</small>
        {% endif %}
      </div>
    </div>
  {% endfor %}
</div>

<div class="category-posts">
  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
    <section class="category-section" id="category-{{ category[0] | slugify }}">
      <h2>{{ category[0] }} <span class="post-count">({{ category[1].size }})</span></h2>
      <div class="posts-grid">
        {% for post in category[1] %}
          <article class="post-card">
            <div class="post-card-header">
              <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
              <time class="post-date">{{ post.date | date: "%Y年%m月%d日" }}</time>
            </div>
            <div class="post-excerpt">
              {{ post.excerpt | strip_html | truncatewords: 20 }}
            </div>
            <div class="post-tags">
              {% for tag in post.tags limit:3 %}
                <span class="tag">{{ tag }}</span>
              {% endfor %}
            </div>
          </article>
        {% endfor %}
      </div>
    </section>
  {% endfor %}
</div>

<style>
/* Additional styles for category page */
.category-preview {
  margin-top: 1rem;
  text-align: left;
  opacity: 0.7;
  font-size: 0.85rem;
  line-height: 1.4;
}

.category-section {
  margin-bottom: 4rem;
}

.category-section h2 {
  border-bottom: 3px solid;
  border-image: var(--gradient-primary) 1;
  padding-bottom: 1rem;
  margin-bottom: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.post-count {
  background: var(--gradient-secondary);
  color: var(--text-light);
  padding: 0.3rem 0.8rem;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 600;
}

.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.post-card {
  background: var(--glass-bg);
  backdrop-filter: blur(15px);
  border: 1px solid var(--glass-border);
  border-radius: 15px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.post-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--gradient-tertiary);
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: -1;
}

.post-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-lg);
}

.post-card:hover::before {
  opacity: 0.05;
}

.post-card-header {
  margin-bottom: 1rem;
}

.post-card h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
}

.post-card h3 a {
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-decoration: none;
  transition: all 0.3s ease;
}

.post-card h3 a:hover {
  background: var(--gradient-secondary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.post-date {
  color: var(--text-secondary);
  font-size: 0.85rem;
  font-weight: 500;
}

.post-excerpt {
  color: var(--text-primary);
  line-height: 1.6;
  margin-bottom: 1rem;
}

.post-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.post-tags .tag {
  background: var(--gradient-tertiary);
  color: var(--text-light);
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 500;
}

/* Quick category navigation */
.category-nav {
  background: var(--glass-bg);
  backdrop-filter: blur(15px);
  border: 1px solid var(--glass-border);
  border-radius: 15px;
  padding: 1.5rem;
  margin-bottom: 3rem;
  text-align: center;
}

.category-nav h3 {
  margin-bottom: 1rem;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.category-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem;
}

.category-links a {
  background: var(--gradient-secondary);
  color: var(--text-light);
  padding: 0.5rem 1rem;
  border-radius: 25px;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.3s ease;
}

.category-links a:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

@media screen and (max-width: 768px) {
  .category-grid {
    grid-template-columns: 1fr;
  }
  
  .posts-grid {
    grid-template-columns: 1fr;
  }
  
  .category-links {
    flex-direction: column;
    align-items: center;
  }
}
</style>

<script>
// Smooth scrolling for category navigation
document.addEventListener('DOMContentLoaded', function() {
  // Add click event to category cards for navigation
  document.querySelectorAll('.category-card').forEach(function(card) {
    card.addEventListener('click', function() {
      const categoryName = this.querySelector('h3').textContent;
      const categoryId = 'category-' + categoryName.toLowerCase().replace(/[^a-z0-9]/g, '-');
      const targetElement = document.getElementById(categoryId);
      if (targetElement) {
        targetElement.scrollIntoView({ behavior: 'smooth' });
      }
    });
    
    // Add cursor pointer style
    card.style.cursor = 'pointer';
  });
});
</script> 