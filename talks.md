---
layout: default
title: Talks
talks:
  -
    title: DevTools at Etsy
    url: https://speakerdeck.com/mrtazz/devtools-at-etsy
    conference: DevOpsDays Berlin
    date: May 27, 2013
  -
    title: Scaling Deployment at Etsy
    url: https://speakerdeck.com/mrtazz/scaling-deployment-at-etsy
    conference: Scaleconf Cape Town
    date: Apr 18, 2013
  -
    title: StatsD Workshop
    url: https://speakerdeck.com/mrtazz/statsd-workshop
    conference: Monitorama Boston
    date: Mar 29, 2013
  -
    title: Chef Workflow at Etsy
    url: https://speakerdeck.com/mrtazz/chef-workflow-at-etsy
    conference: NYC Chef Meetup
    date: Jan 29, 2013
---
## [Talks]({{page.url}})

<div class="postconent talks">
  <ul class="archive">
  {% for talk in page.talks %}
    <li>
    <a href="{{ talk.url }}"> {{ talk.title }}, {{ talk.conference }}</a>
    <span class="archivedate">{{ talk.date }}</span>
    </li>
  {% endfor %}
  </ul>
</div>
