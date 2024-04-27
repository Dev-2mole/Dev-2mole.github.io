---
layout: archive
title: "대외활동"
permalink: /activity/
---
<div class="tiles">
{% for post in site.posts %}
	{% if post.categories contains 'activity' %}
    {% include post-grid.html %}
  {% endif %}
{% endfor %}
</div><!-- /.tiles -->
