---
layout: content
title:  "�j���[�X�^News"
categories: tcpreplay wiki
description: "Tcpreplay �j���[�X�^Tcpreplay news"
---

<div id="home">
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>
