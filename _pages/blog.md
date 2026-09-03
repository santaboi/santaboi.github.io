---
layout: page
permalink: /blog/
title: Random Thoughts
nav: true
nav_order: 3
---

<div class="blog-intro mb-4">
  <p class="text-muted">All thought on my own.</p>
  <div class="d-flex flex-wrap blog-taxonomy">
    {% if site.display_tags %}
      <div class="mr-3">
        <span class="font-weight-bold">Tags:</span>
        {% for tag in site.display_tags %}
          {% assign tag_slug = tag | slugify %}
          <a class="badge badge-pill badge-light" href="{{ '/blog/tag/' | append: tag_slug | append: '/' | relative_url }}">{{ tag }}</a>
        {% endfor %}
      </div>
    {% endif %}
    {% if site.display_categories %}
      <div>
        <span class="font-weight-bold">Categories:</span>
        {% for category in site.display_categories %}
          {% assign cat_slug = category | slugify %}
          <a class="badge badge-pill badge-light" href="{{ '/blog/category/' | append: cat_slug | append: '/' | relative_url }}">{{ category }}</a>
        {% endfor %}
      </div>
    {% endif %}
  </div>
</div>

{% assign blog_include_only = site.latest_posts.blogpage_include_only %}
{% assign blog_posts = site.posts %}
{% if blog_include_only and blog_include_only != empty %}
{% assign blog_posts = blog_posts | where_exp: 'post', 'blog_include_only contains post.path' %}
{% endif %}
{% assign blog_posts_count = blog_posts | size %}
{% if blog_posts_count == 0 %}

  <div class="alert alert-info" role="alert">No posts. Come back later!</div>
{% else %}
  {% assign posts_by_year = blog_posts | group_by_exp: 'post', "post.date | date: '%Y'" %}
  {% assign posts_by_year = posts_by_year | sort: 'name' | reverse %}
  {% for year in posts_by_year %}
    <section class="mb-5">
      <h2 class="font-weight-bold">{{ year.name }}</h2>
      <div class="table-responsive">
        <table class="table table-sm table-borderless">
          {% for post in year.items %}
            <tr>
              <th scope="row" style="width: 20%">{{ post.date | date: '%b %d' }}</th>
              <td>
                {% if post.redirect == blank %}
                  <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
                {% elsif post.redirect contains '://' %}
                  <a class="post-link" href="{{ post.redirect }}" target="_blank">{{ post.title }}</a>
                {% else %}
                  <a class="post-link" href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
                {% endif %}
                {% if post.description %}
                  <div class="text-muted small">{{ post.description }}</div>
                {% endif %}
              </td>
            </tr>
          {% endfor %}
        </table>
      </div>
    </section>
  {% endfor %}
{% endif %}
