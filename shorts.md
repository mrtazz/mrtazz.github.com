---
layout: default
title: Short Bits
---
## [Short Bits]({{page.url}})
<div class="postcontent archive">
  <ul class="archive">
    {% for post in site.categories.bits %}
      <li>
      <a href="{{ post.url }}"> {{ post.title }}</a>
      <span class="archivedate">{{ post.date | date: "%b %d, %Y"}}</span>
      </li>
    {% endfor %}
  </ul>
</div>
