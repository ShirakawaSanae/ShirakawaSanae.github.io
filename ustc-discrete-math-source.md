---
layout: null
title: "USTC Discrete Mathematics Resources"
permalink: /ustc-discrete-math-source/
sitemap: false
---

<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="robots" content="noindex, nofollow">
  <title>{{ page.title }}</title>
  <style>
    :root {
      color: #20323a;
      background: #fcfcfa;
      font-family: "Times New Roman", "Songti SC", "SimSun", serif;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: #fcfcfa;
    }

    .course-page {
      width: min(100% - 2.5rem, 48rem);
      margin: 0 auto;
      padding: 4.5rem 0 3rem;
    }

    .course-resource-intro {
      margin-bottom: 2.4rem;
      padding: 1.25rem 1.4rem;
      border-left: 3px solid #006b70;
      background: #eef5f4;
    }

    .course-resource-kicker {
      margin: 0 0 0.5rem;
      color: #006b70;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      font-size: 0.68rem;
      font-weight: 700;
    }

    h1 {
      margin: 0;
      color: #20323a;
      font-size: 1.5rem;
      font-weight: 600;
      line-height: 1.3;
    }

    .course-resource-intro p:not(.course-resource-kicker) {
      margin: 0.7rem 0 0;
      font-size: 0.9rem;
      line-height: 1.65;
    }

    .course-resource-note { color: #5d6970; }
    .course-resource-contact { color: #8f332b; }
    a { color: #006b70; }

    .course-resource-section + .course-resource-section {
      margin-top: 2.2rem;
    }

    .course-resource-section h2 {
      margin: 0;
      padding-bottom: 0.45rem;
      border-bottom: 1px solid #20323a;
      color: #20323a;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      font-size: 1rem;
      font-weight: 650;
      line-height: 1.35;
    }

    .course-resource-heading p {
      margin: 0.55rem 0 0;
      color: #5d6970;
      font-size: 0.82rem;
      line-height: 1.55;
    }

    .course-resource-list {
      margin: 0;
      padding: 0;
      list-style: none;
    }

    .course-resource-item {
      padding: 0.85rem 0;
      border-bottom: 1px solid #d6dde0;
    }

    .course-resource-item a {
      font-size: 0.92rem;
      font-weight: 700;
      line-height: 1.45;
    }

    .course-resource-item p {
      margin: 0.2rem 0 0;
      color: #5d6970;
      font-size: 0.8rem;
      line-height: 1.55;
    }

    .course-resource-empty {
      margin: 0;
      padding: 0.8rem 0;
      border-bottom: 1px solid #d6dde0;
      color: #5d6970;
      font-size: 0.82rem;
    }

    .course-resource-updated {
      margin: 3rem 0 0;
      color: #5d6970;
      font-size: 0.76rem;
    }

    @media (max-width: 36rem) {
      .course-page {
        width: min(100% - 1.5rem, 48rem);
        padding-top: 2rem;
      }
    }
  </style>
</head>
<body>
  <main class="course-page">
    <header class="course-resource-intro">
      <p class="course-resource-kicker">USTC / COMPUTER SCIENCE</p>
      <h1>离散数学资料</h1>
      <p>USTC 本科教学之计科离散数学三部曲的课程资料索引。所有资料均来自公开页面或资料作者本人授权；请直接使用本页 URL 分享资料。</p>
      <p class="course-resource-contact">如需更新资料，请联系 <a href="mailto:sunj1ngwen@mail.ustc.edu.cn">sunj1ngwen@mail.ustc.edu.cn</a>。</p>
      {% if site.data.ustc_discrete_math_resources.note %}<p class="course-resource-note">{{ site.data.ustc_discrete_math_resources.note }}</p>{% endif %}
    </header>

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
          <a href="{% if item.url contains '://' %}{{ item.url }}{% else %}{{ item.url | relative_url }}{% endif %}"{% if item.url contains '://' %} rel="noopener" target="_blank"{% endif %}>{{ item.title }}</a>
          {% if item.description %}<p>{{ item.description }}</p>{% endif %}
        </li>
        {% endfor %}
      </ul>
      {% else %}
      <p class="course-resource-empty">资料整理中。</p>
      {% endif %}
    </section>
    {% endfor %}

    <p class="course-resource-updated">最后更新：{{ site.time | date: "%Y-%m-%d" }}</p>
  </main>
</body>
</html>
