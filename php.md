---
title: PHP
permalink: "/php/"
position: 0
layout: page
---

{% for tag in site.categories %}
{% if tag[0] == "php" %}
  <ul class="collection">
    {% assign pages_list = tag[1] %}
    {% for post in pages_list %}
      {% if post.title != null %}
      {% if group == null or group == post.group %}


     <div class="col s6 m4  collection-item hoverable">



{% assign date_format = site.minima.date_format | default: "%-d %b %Y" %}<div>
<time datetime="{{ post.date | date: date_format }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></div>
<span class="title">
<a class="post-link" href="{{ site.url }}{{ post.url }}">{{ post.title | escape }}</a>
</span>
<p>
             {{ post.content |strip_html | truncatewords: 100 }}
             
          </p>
 <p> <a href="{{ post.url | relative_url }}" class="btn light-blue">Read More</a></p>
</div>


      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
{% endif %}
{% endfor %}