---
layout: single
title: "Blog"
permalink: /blog/
author_profile: true
classes: wide
---

Notes on research, reading, and thinking.

<div class="topic-grid">
{% for topic in site.data.blog_topics %}
  {% capture topic_url %}/blog/{{ topic.slug }}/{% endcapture %}
  <article class="topic-item">
    <h2><a href="{{ topic_url | relative_url }}">{{ topic.title }}</a></h2>
    <p>{{ topic.description }}</p>
    <a class="topic-visit" href="{{ topic_url | relative_url }}">Browse topic</a>
  </article>
{% endfor %}
</div>

## Latest Notes

<div class="post-list">
{% for post in site.posts %}
  <article class="post-item">
    <p class="post-date">{{ post.date | date: "%B %-d, %Y" }}</p>
    <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
    {% if post.excerpt %}<p>{{ post.excerpt | markdownify | strip_html | truncate: 220 }}</p>{% endif %}
    <a class="post-read" href="{{ post.url | relative_url }}">Read note</a>
  </article>
{% else %}
  <p>No public posts yet.</p>
{% endfor %}
</div>
