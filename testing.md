---
layout: archive
title: "테스트용"
permalink: /TESTING/
---
<div class="tiles">
{% for post in site.posts %}
	{% if post.categories contains 'TESTING' %}
    {% include post-grid.html %}
  {% endif %}
{% endfor %}
</div><!-- /.tiles -->
