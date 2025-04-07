---
layout: page
title: Blog
permalink: /blog/
---

<div class="blog-archive">
  <p>Welcome to my blog where I share insights about backend engineering, system architecture, and software development.</p>

  {%- if site.posts.size > 0 -%}
    <h2 class="post-list-heading">{{ page.list_title | default: "All Posts" }}</h2>
    <ul class="post-list">
      {%- for post in site.posts -%}
      <li>
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
        
        {%- if post.tags.size > 0 -%}
        <div class="post-tags">
          <span>Tags: </span>
          {%- for tag in post.tags -%}
          <span class="post-tag" style="display: inline-block; background-color: #f0f0f0; padding: 2px 8px; margin-right: 5px; border-radius: 3px; font-size: 0.8em;">{{ tag }}</span>
          {%- endfor -%}
        </div>
        {%- endif -%}
      </li>
      {%- endfor -%}
    </ul>
  {%- endif -%}
</div>
