---
layout: page
title: Work
subtitle: Project logs, operating notes, and technical snapshots.
permalink: /work/
description: Work-related posts and test entries for Patrick Li.
---

{% assign work_items = site.work | sort: "date" | reverse %}

<section class="panel">
  <p class="section-label">Collection</p>
  <p class="lead">
    This page is reserved for professional work, structured notes, and project summaries.
    It starts with a seed entry so the section has a clean shape to build on.
  </p>
</section>

<section class="section-block">
  <div class="section-heading">
    <h2>Entries</h2>
  </div>

  <div class="article-list">
    {% for item in work_items %}
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

    {% if work_items.size == 0 %}
      <div class="empty-state">No work entries yet.</div>
    {% endif %}
  </div>
</section>
