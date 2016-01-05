---
layout: default
title: Blog
permalink: /blog/
---

<div class="home">

  <!--
  <h1 class="page-heading">Posts</h1>
  -->

  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <header class="post-header">
          <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
          <h1 class="post-title">
            <a class="post-link" href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
          </h1>
          <span class="post-meta"><!--{% if post.author %} - {{ post.author }}{% endif %}-->{% if post.meta %}{{ post.meta }}{% endif %}</span>
        </header>

        <span class="post-meta"><p>{{ post.excerpt | strip_html }}...</p></span>
        <span class="post-meta">{% for category in post.categories %}
          <a href="{{ site.baseurl }}/posts/#{{ category }}">{{ category }}</a>
        {% endfor %}</span>
      </li>
    {% endfor %}
  </ul>

  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

</div>
