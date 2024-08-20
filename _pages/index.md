---
layout: page
title: Home
id: home
permalink: /
---

# Welcome! 🌱

<p style="padding: 3em 1em; background: #f5f7ff; border-radius: 4px;">
  안녕하세요 seongwol 입니다.<br>
  프로젝트 시작 페이지 문서입니다.<br>
  배포도 처음 해보고 웹도 잘 몰라서 <br>
  일단 옵시디언 html 변환 예시 템플릿에 넣은거라 많이 조잡합니다...<br>
  일단 목표는 과제 번역 / 링크 및 해당 사항 공부라서 크게 안건들고 띄웠습니다.<br>
  같이 보고 잘 해봐요<br>
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
