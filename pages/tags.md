---
layout: page
title: Tags
permalink: /tags/
---

{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign tag_words = site_tags | split:',' | sort %}

<div class="tags-expo">
  <div class="tags-expo-list">
    {% for tag in tag_words %}
    <a href="#{{ tag | slugify }}" class="post-tag" style="display: inline-block; background-color: #f0f0f0; padding: 4px 10px; margin: 5px; border-radius: 3px; text-decoration: none;">{{ tag }}</a>
    {% endfor %}
  </div>
  
  <hr/>
  
  <div class="tags-expo-section">
    {% for tag in tag_words %}
    <h2 id="{{ tag | slugify }}">{{ tag }}</h2>
    <ul class="tags-expo-posts">
      {% for post in site.tags[tag] %}
      <li>
        <a class="post-link" href="{{ site.baseurl }}{{ post.url }}">
          {{ post.title }}
        </a>
        <small class="post-date">{{ post.date | date_to_string }}</small>
      </li>
      {% endfor %}
    </ul>
    {% endfor %}
  </div>
</div>
