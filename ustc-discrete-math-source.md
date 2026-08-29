---
layout: single
title: "USTC Discrete Mathematics Resources"
permalink: /ustc-discrete-math-source/
author_profile: false
share: false
related: false
sitemap: false
robots: noindex, nofollow
classes: wide
---

<div class="course-resource-intro">
  <p class="course-resource-kicker">COURSE MATERIALS</p>
  <p>USTC本科教学之计科离散数学三部曲的课程资料的公开索引，所有资料均来自于公开页面或资料作者本人授权。如需分享，请直接使用本页 URL 分享资料。</p>
  <p><span style="color: #8f332b;">如需更新资料，请联系 <a href="mailto:sunj1ngwen@mail.ustc.edu.cn">sunj1ngwen@mail.ustc.edu.cn</a>。</span></p>
  {% if site.data.ustc_discrete_math_resources.note %}<p class="course-resource-note">{{ site.data.ustc_discrete_math_resources.note }}</p>{% endif %}
</div>

{% for section in site.data.ustc_discrete_math_resources.sections %}
<section class="course-resource-section">
  <div class="course-resource-heading">
    <h2>{{ section.title }}</h2>
    {% if section.description %}<p>{{ section.description }}</p>{% endif %}
  </div>
  {% if section.items and section.items.size > 0 %}
  <ul class="course-resource-list">
    {% for item in section.items %}
    <li class="course-resource-item">
      <div>
        <a href="{% if item.url contains '://' %}{{ item.url }}{% else %}{{ item.url | relative_url }}{% endif %}"{% if item.url contains '://' %} rel="noopener" target="_blank"{% endif %}>{{ item.title }}</a>
        {% if item.description %}<p>{{ item.description }}</p>{% endif %}
      </div>
      {% if item.type %}<span class="course-resource-type">{{ item.type }}</span>{% endif %}
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p class="course-resource-empty">资料整理中。</p>
  {% endif %}
</section>
{% endfor %}

<p class="course-resource-updated">最后更新：{{ site.time | date: "%Y-%m-%d" }}</p>
