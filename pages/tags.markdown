---
layout: page
title: Tags
permalink: /tags/
---

<div class="tags-container">
  <h1>Tags</h1>
  
  <div class="tags-list">
    {% assign all_tags = site.posts | map: 'tags' | flatten | uniq | sort %}
    {% for tag in all_tags %}
      <a href="#{{ tag | slugify }}" class="tag-link">{{ tag }}</a>
    {% endfor %}
  </div>

  <div class="tagged-posts">
    {% for tag in all_tags %}
      <div id="{{ tag | slugify }}" class="tag-section">
        <h2>{{ tag }}</h2>
        <div class="tag-posts-grid">
          {% for post in site.posts %}
            {% if post.tags contains tag %}
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
                <div class="post-tags">
                  {% for post_tag in post.tags %}
                    {% if post_tag != tag %}
                      <a href="/tags/#{{ post_tag | slugify }}" class="post-tag">{{ post_tag }}</a>
                    {% endif %}
                  {% endfor %}
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
  .tags-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }

  .tags-list {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 2rem;
    padding: 1rem;
    background-color: #f8f9fa;
    border-radius: 8px;
  }

  .tag-link {
    display: inline-block;
    padding: 0.5rem 1rem;
    background-color: white;
    color: #4056A1;
    text-decoration: none;
    border-radius: 4px;
    transition: all 0.3s;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }

  .tag-link:hover {
    background-color: #4056A1;
    color: white;
    transform: translateY(-2px);
  }

  .tag-section {
    margin-bottom: 3rem;
    background-color: white;
    border-radius: 8px;
    padding: 2rem;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }

  .tag-section h2 {
    color: #4056A1;
    border-bottom: 2px solid #4056A1;
    padding-bottom: 0.5rem;
    margin-bottom: 1.5rem;
    display: inline-block;
  }

  .tag-posts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1.5rem;
  }

  .post-card {
    background-color: #f8f9fa;
    padding: 1.5rem;
    border-radius: 8px;
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
    font-size: 1.1rem;
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
    font-size: 0.95rem;
  }

  .post-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 1rem;
  }

  .post-tag {
    display: inline-block;
    padding: 0.25rem 0.75rem;
    background-color: white;
    color: #4056A1;
    text-decoration: none;
    border-radius: 4px;
    font-size: 0.8rem;
    transition: all 0.3s;
  }

  .post-tag:hover {
    background-color: #4056A1;
    color: white;
  }

  @media (max-width: 768px) {
    .tags-container {
      padding: 1rem;
    }

    .tag-posts-grid {
      grid-template-columns: 1fr;
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