---
layout: default
title: writing
---
## [Writing]({{page.url}})
<div class="postcontent archive">
  <ul class="archive">
  {% for post in site.posts %}
    {% unless post.categories.first == "bits"  %}
      <li>
      <a href="{{ post.url }}"> {{ post.title }}</a>
      <span class="archivedate hidemobile">{{ post.date | date: "%b %d, %Y"}}</span>
      </li>
    {% endunless %}
  {% endfor %}
  </ul>
</div>
