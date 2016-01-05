---
layout: page
title: Posts
permalink: /posts/
---

<h3>Projects</h3>

{% for tag in site.categories %} 
  <h4 class="category-header" id="p_{{ tag[0] }}">{{ tag[0] }}</h4>
  <ul class="cat-links">
    {% assign pages_list = tag[1] %}  
    {% for post in pages_list %}
      {% if post.title != null %}
      {% if group == null or group == post.group %}
      <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span> &#8212; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
  	  </li>
      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
{% endfor %}

<h3>Tags</h3>

{% for tag in site.tags %} 
  <h4 class="category-header" id="t_{{ tag[0] }}">{{ tag[0] }}</h4>
  <ul class="cat-links">
    {% assign pages_list = tag[1] %}  
    {% for post in pages_list %}
      {% if post.title != null %}
      {% if group == null or group == post.group %}
      <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span> &#8212; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      </li>
      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
{% endfor %}