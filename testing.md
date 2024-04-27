---
layout: archive
title: "스터디"
permalink: /STUDY/
---
<div class="tiles">
<h4>공부한 내용을 정리해서 먼저 올리는 페이지 입니다. <br>
리스트 정리는 추후에 할 예정입니다.
{% for post in site.posts %}
	{% if post.categories contains 'STUDY' %}
    {% include post-grid.html %}
  {% endif %}
{% endfor %}
