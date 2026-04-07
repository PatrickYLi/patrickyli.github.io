---
layout: page
title: Music Projects
subtitle: Releases, sketches, and experiments collected as a clean list.
permalink: /music/
description: Music projects and test entries for Patrick Li.
---

{% assign music_items = site.music | sort: "date" | reverse %}

<section class="panel">
  <p class="section-label">Collection</p>
  <p class="lead">
    This page is the dedicated home for music-related posts. It is intentionally simple:
    a scrolling list of entries you can expand one by one.
  </p>
</section>

<section class="section-block">
  <div class="section-heading">
    <h2>Entries</h2>
  </div>

  <div class="article-list">
    {% for item in music_items %}
      <article class="entry-card">
        <p class="entry-meta">
          {{ item.date | date: "%Y-%m-%d" }}
          {% if item.status %} // {{ item.status }}{% endif %}
        </p>
        <h2><a href="{{ item.url | relative_url }}">{{ item.title }}</a></h2>
        {% if item.subtitle %}
          <p>{{ item.subtitle }}</p>
        {% endif %}
        <p>{{ item.excerpt | strip_html | truncatewords: 30 }}</p>
        <p class="read-hint"><a href="{{ item.url | relative_url }}">Read entry</a></p>
      </article>
    {% endfor %}

    {% if music_items.size == 0 %}
      <div class="empty-state">No music entries yet.</div>
    {% endif %}
  </div>
</section>
