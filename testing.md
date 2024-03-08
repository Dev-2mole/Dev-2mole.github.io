---
layout: archive
title: "스터디"
permalink: /STUDY/
---
<div class="tiles">
<h4>공부한 내용을 정리해서 올리는 페이지 입니다. 리스트 정리는 추후에 모아둘 예정입니다.<h4>
{% for post in site.posts %}
	{% if post.categories contains 'STUDY' %}
    {% include post-grid.html %}
  {% endif %}
{% endfor %}
</div><!-- /.tiles -->
