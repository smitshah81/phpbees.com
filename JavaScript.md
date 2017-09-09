---
title: JavaScript
permalink: "/javascript/"
position: 3
layout: page
---

<div class="collection">
        {% for post in taxonomy['categories']['test'] %}
        <div class="col s6 m4  collection-item hoverable">
          {% assign date_format = site.minima.date_format | default: "%-d %b %Y" %}
          <div class="">{{ post.date | date: date_format }}</div>
          <span class="title"><a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></span>
         
          <p>
             {{ post.content |strip_html | truncatewords: 100 }}
             
          </p>
         <p> <a href="{{ post.url | relative_url }}" class="btn light-blue">Read More</a></p>
        </div>
        
        {% endfor %}
        </div>  