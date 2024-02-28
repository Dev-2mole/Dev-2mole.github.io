---
layout: archive
title: "대외활동"
permalink: /ACTIVITY/
---
<div class="tiles">
{% for post in site.posts %}
	{% if post.categories contains 'ACTIVITY' %}
    {% include post-grid.html %}
  {% endif %}
{% endfor %}
</div><!-- /.tiles -->
