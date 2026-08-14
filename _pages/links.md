---
layout: single
title: "Friend Links"
permalink: /links/
author_profile: true
classes: wide
---

<ul class="academic-list friend-list">
{% for friend in site.data.friends %}
  <li class="friend-item"><strong><a href="{{ friend.url }}" rel="noopener" target="_blank">{{ friend.name }}</a></strong><span class="friend-description"> - {{ friend.description }}</span></li>
{% endfor %}
</ul>
