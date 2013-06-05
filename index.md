---
layout: default
title: writing
---
## [Writing]({{page.url}})
<div class="postcontent archive">
  <ul class="archive">
  {% for post in site.posts %}
    <li>
    <a href="{{ post.url }}"> {{ post.title }}</a>
    <span class="archivedate">{{ post.date | date: "%b %d, %Y"}}</span>
    </li>
  {% endfor %}
  </ul>
</div>
