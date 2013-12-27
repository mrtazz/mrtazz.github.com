---
layout: default
title: Talks
---
## [Talks]({{page.url}})

<div class="postconent talks">
  <ul class="archive">
  {% for talk in site.data.talks %}
    <li>
    {{ talk.title }}, {{ talk.conference }}
    <a href="{{ talk.url }}"><i class="icon-edit" style="font-size: 15px;"> </i></a>
    {% if talk.video %}
    <a href="{{ talk.video }}"><i class="icon-film" style="font-size: 15px;"> </i></a>
    {% endif %}
    <span class="archivedate">{{ talk.date }}</span>
    </li>
  {% endfor %}
  </ul>
</div>
