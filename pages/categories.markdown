---
layout: page
title: Categories
permalink: /categories/
---

<div class="categories-container">
  <header class="categories-header">
    <h1>Categories</h1>
    <p class="subtitle">Browse posts by category</p>
  </header>

  <div class="categories-list">
    {% assign categories = site.posts | map: 'categories' | flatten | uniq | sort %}
    {% for category in categories %}
      <div id="{{ category | slugify }}" class="category-section">
        <h2 class="category-title">
          <span class="category-icon">📁</span>
          {{ category }}
        </h2>
        <div class="posts-grid">
          {% for post in site.posts %}
            {% if post.categories contains category %}
              <div class="post-card">
                <div class="post-card-header">
                  <h3>
                    <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
                  </h3>
                  <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
                </div>
                {% if post.description %}
                  <p class="post-description">{{ post.description }}</p>
                {% endif %}
                <div class="post-meta">
                  {% if post.tags.size > 0 %}
                    <div class="post-tags">
                      {% for tag in post.tags %}
                        <a href="/tags/#{{ tag | slugify }}" class="post-tag">#{{ tag }}</a>
                      {% endfor %}
                    </div>
                  {% endif %}
                </div>
              </div>
            {% endif %}
          {% endfor %}
        </div>
      </div>
    {% endfor %}
  </div>
</div>

<style>
  .categories-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }

  .categories-header {
    text-align: center;
    margin-bottom: 3rem;
  }

  .categories-header h1 {
    color: #4056A1;
    margin-bottom: 0.5rem;
  }

  .subtitle {
    color: #666;
    font-size: 1.2rem;
    margin-top: 0;
  }

  .category-section {
    margin-bottom: 3rem;
  }

  .category-title {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: #4056A1;
    margin-bottom: 1.5rem;
    padding-bottom: 0.5rem;
    border-bottom: 2px solid #f0f0f0;
  }

  .category-icon {
    font-size: 1.2rem;
  }

  .posts-grid {
    display: grid;
    gap: 2rem;
  }

  .post-card {
    background-color: white;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    transition: all 0.3s;
  }

  .post-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }

  .post-card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 1rem;
  }

  .post-link {
    color: #333;
    text-decoration: none;
    font-size: 1.25rem;
    font-weight: 500;
  }

  .post-link:hover {
    color: #4056A1;
  }

  .post-date {
    color: #666;
    font-size: 0.9rem;
    white-space: nowrap;
    margin-left: 1rem;
  }

  .post-description {
    color: #666;
    margin: 1rem 0;
    line-height: 1.5;
    font-size: 1rem;
  }

  .post-meta {
    margin-top: 1.5rem;
  }

  .post-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .post-tag {
    display: inline-block;
    padding: 0.5rem 1rem;
    background-color: #f8f9fa;
    color: #666;
    text-decoration: none;
    border-radius: 4px;
    font-size: 0.9rem;
    transition: all 0.3s;
  }

  .post-tag:hover {
    background-color: #e9ecef;
    color: #333;
  }

  @media (max-width: 768px) {
    .categories-container {
      padding: 1rem;
    }

    .post-card {
      padding: 1.5rem;
    }

    .post-card-header {
      flex-direction: column;
    }

    .post-date {
      margin-left: 0;
      margin-top: 0.5rem;
    }
  }
</style> 