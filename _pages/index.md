---
layout: page
title: Home
id: home
permalink: /
---

# Welcome! 🌱

<p style="padding: 3em 1em; background: #f5f7ff; border-radius: 4px;">
  시작 문서입니다.<br>
  배포도 처음 해보고 잘 몰라서 템플릿에 넣은거라 뭔가 불편할순있지만...<br>
  공부한 걸 저장해놨으니 같이 보고 잘 해봐요<br>
  <span style="font-weight: bold"><a href="{{ site.baseurl }}/ft-transcendance">ft_transcendance</a></span>
</p>

<strong>Recently updated notes</strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 5 %}
    <li>
      {{ note.last_modified_at | date: "%Y-%m-%d" }} — <a class="internal-link" href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a>
    </li>
  {% endfor %}
</ul>

<style>
  .wrapper {
    max-width: 46em;
  }
</style>
