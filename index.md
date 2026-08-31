---
layout: single
title: "About Me"
author_profile: true
permalink: /
classes: wide
---

I am **Jingwen Sun**, a graduate student at the School of Computer Science, University of Science and Technology of China(USTC). My research focus on AI infra and machine learning system. I am currently a member at [USTC MLSys Lab](https://ustc-mlsys.github.io), under the supervision of [Cheng Li](https://mr-cheng-li.github.io/) and [Youhui Bai](https://youhuibai.github.io/). I hope to do some safe, effective and elegant work :D . I like hiking, reading, photography, and volunteer service.



## Research Interests

<ul class="academic-list interest-list">
  <li><strong>Distributed LLM training and inference systems</strong>.</li>
  <li><strong>Optimization and fault-tolerance framework of large-scale parallel systems</strong>.</li>
  <li><strong>AI agents</strong>.</li>
</ul>

## Publications

<div class="publication-list">
{% for publication in site.data.publications %}
  <article class="publication-item">
    <div class="publication-labels" aria-label="Publication details">
      <span class="publication-venue-tag">{{ publication.venue }}</span>
      {% if publication.note %}<span class="publication-status-tag">{{ publication.note }}</span>{% endif %}
    </div>
    <div class="publication-body">
      <h3 class="publication-title">{{ publication.title }}</h3>
      <p class="publication-authors">{{ publication.authors }}</p>
      <p class="publication-meta"><em>{{ publication.venue }}</em>, {{ publication.year }}</p>
      <p class="publication-links">{% if publication.paper %}<a href="{{ publication.paper }}"{% if publication.paper contains '://' %} rel="noopener" target="_blank"{% endif %}>Paper</a>{% endif %}{% if publication.code %}<a href="{{ publication.code }}"{% if publication.code contains '://' %} rel="noopener" target="_blank"{% endif %}>Code</a>{% endif %}</p>
    </div>
  </article>
{% endfor %}
</div>


## Projects

<ul class="academic-list project-list">
{% for project in site.data.projects %}
  <li class="project-item">
    {% if project.url %}
    <strong class="project-title"><a href="{{ project.url }}"{% if project.url contains '://' %} rel="noopener" target="_blank"{% endif %}>{{ project.title }}</a></strong>
    {% else %}
    <strong class="project-title">{{ project.title }}</strong>
    {% endif %}
    <span class="project-summary"> - {{ project.summary }}</span>
    <span class="project-tags" aria-label="Project tags">{% assign project_tags = project.tags | split: ',' %}{% for tag in project_tags %}<span class="project-tag">{{ tag | strip }}</span>{% endfor %}</span>
  </li>
{% endfor %}
</ul>

## Contests and Awards

<ul class="academic-list project-list">
{% for _item in site.data.contests_awards %}
  <li class="project-item">
    {% if _item.url %}
    <strong class="project-title"><a href="{{ _item.url }}"{% if _item.url contains '://' %} rel="noopener" target="_blank"{% endif %}>{{ _item.title }}</a></strong>
    {% else %}
    <strong class="project-title">{{ _item.title }}</strong>
    {% endif %}
    <span class="project-summary"> - {{ _item.summary }}</span>
    <span class="project-tags" aria-label="Project tags">{% assign project_tags = _item.tags | split: ',' %}{% for tag in project_tags %}<span class="project-tag">{{ tag | strip }}</span>{% endfor %}</span>
  </li>
{% endfor %}
</ul>

## Extracurricular Commitment

<ul class="academic-list project-list">
{% for _item in site.data.commitment %}
  <li class="project-item">
    {% if _item.url %}
    <strong class="project-title"><a href="{{ _item.url }}"{% if _item.url contains '://' %} rel="noopener" target="_blank"{% endif %}>{{ _item.title }}</a></strong>
    {% else %}
    <strong class="project-title">{{ _item.title }}</strong>
    {% endif %}
    <span class="project-summary"> - {{ _item.summary }}</span>
    <span class="project-tags" aria-label="Project tags">{% assign project_tags = _item.tags | split: ',' %}{% for tag in project_tags %}<span class="project-tag">{{ tag | strip }}</span>{% endfor %}</span>
  </li>
{% endfor %}
</ul>

## Links and Blogs

<p class="section-action"><a href="{{ '/links/' | relative_url }}">Friends, labs, and collaborators</a></p>

<p class="section-action"><a href="{{ '/blog/' | relative_url }}">Reading and research blogs</a></p>
