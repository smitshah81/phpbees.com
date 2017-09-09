---
title: Search
date: 2017-09-09 14:38:00 +05:30
permalink: "/tags"
layout: tag
---

<ul>
  {% for tag in page.tags %}
    <li>
      <a href="/{{ site.tag_page_dir }}/{{ tag | slugify: 'pretty' }}/">{{ tag }}</a>
    </li>
  {% endfor %}
</ul>