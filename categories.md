---
layout: default
title: 分类
permalink: /categories/
---

<div class="category-header">
  <h1>📂 文章分类</h1>
  <p>探索不同主题的精彩内容，找到你感兴趣的文章</p>
</div>

<!-- Quick navigation to categories -->
<div class="category-quick-nav">
  <h3>🔍 快速跳转</h3>
  <div class="category-links">
    {% assign sorted_categories = site.categories | sort %}
    {% for category in sorted_categories %}
      <a href="#category-{{ category[0] | slugify }}">{{ category[0] }} ({{ category[1].size }})</a>
    {% endfor %}
  </div>
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
  border-bottom: 3px solid var(--primary-color);
  padding-bottom: 1rem;
  margin-bottom: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  color: var(--text-primary);
}

.post-count {
  background: var(--primary-color);
  color: var(--text-white);
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
  background: var(--bg-primary);
  border: 1px solid var(--border-light);
  border-radius: 15px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-sm);
}

.post-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-lg);
  border-color: var(--border-medium);
}

.post-card-header {
  margin-bottom: 1rem;
}

.post-card h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
}

.post-card h3 a {
  color: var(--text-primary);
  text-decoration: none;
  transition: all 0.3s ease;
}

.post-card h3 a:hover {
  color: var(--primary-color);
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
  background: var(--bg-accent);
  color: var(--text-secondary);
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 500;
}

.post-tags .tag:hover {
  background: var(--primary-color);
  color: var(--text-white);
}

/* Make category cards clickable for navigation to posts list */
.category-card {
  cursor: pointer;
  transition: all 0.3s ease;
}

.category-card:hover {
  transform: translateY(-2px);
}

/* Quick navigation between categories */
.category-quick-nav {
  background: var(--bg-accent);
  border-radius: 15px;
  padding: 1.5rem;
  margin-bottom: 3rem;
  text-align: center;
}

.category-quick-nav h3 {
  margin-bottom: 1rem;
  color: var(--text-primary);
}

.category-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem;
}

.category-links a {
  background: var(--primary-color);
  color: var(--text-white);
  padding: 0.5rem 1rem;
  border-radius: 25px;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.3s ease;
}

.category-links a:hover {
  background: var(--primary-dark);
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
  
  .post-card {
    padding: 1rem;
  }
}
</style>

<script>
// Enhanced category navigation
document.addEventListener('DOMContentLoaded', function() {
  // Add click event to category cards for navigation
  document.querySelectorAll('.category-card').forEach(function(card) {
    card.addEventListener('click', function() {
      const categoryName = this.querySelector('h3').textContent;
      const categoryId = 'category-' + categoryName.toLowerCase().replace(/[^a-z0-9\u4e00-\u9fff]/g, '-');
      const targetElement = document.getElementById(categoryId);
      if (targetElement) {
        targetElement.scrollIntoView({ behavior: 'smooth', block: 'start' });
        // Add highlight effect
        targetElement.style.backgroundColor = 'var(--bg-accent)';
        targetElement.style.borderRadius = '0.75rem';
        targetElement.style.padding = '1rem';
        targetElement.style.transition = 'all 0.3s ease';
        
        // Remove highlight after animation
        setTimeout(function() {
          targetElement.style.backgroundColor = '';
          targetElement.style.padding = '';
        }, 2000);
      }
    });
  });
  
  // Add smooth scrolling for quick navigation links
  document.querySelectorAll('.category-links a').forEach(function(link) {
    link.addEventListener('click', function(e) {
      e.preventDefault();
      const targetId = this.getAttribute('href').substring(1);
      const targetElement = document.getElementById(targetId);
      if (targetElement) {
        targetElement.scrollIntoView({ behavior: 'smooth', block: 'start' });
        // Add highlight effect
        targetElement.style.backgroundColor = 'var(--bg-accent)';
        targetElement.style.borderRadius = '0.75rem';
        targetElement.style.padding = '1rem';
        targetElement.style.transition = 'all 0.3s ease';
        
        // Remove highlight after animation
        setTimeout(function() {
          targetElement.style.backgroundColor = '';
          targetElement.style.padding = '';
        }, 2000);
      }
    });
  });
});
</script> 