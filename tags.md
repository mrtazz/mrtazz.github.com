---
layout: default
title: Tags
---
## [Tags]({{page.url}})
{% for tagposts in site.tags %}
  <h3 class="tagheading"><a name="{{ tagposts | first}}">{{ tagposts | first}}</a></h3>
  <ul class="tagposts">
    {% for post in tagposts[1] %}
<li>
<a href="{{ post.url }}"> {{ post.title }}</a>
<span class="archivedate">{{ post.date | date: "%b %d, %Y"}}</span>
</li>
    {% endfor %}
  </ul>
{% endfor %}
