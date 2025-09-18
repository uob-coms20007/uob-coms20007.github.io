---
layout: default
title: Detailed Schedule
nav_order: 2
---

{% assign lecture_one_day = "Monday" %}
{% assign lecture_one_time = "3pm" %}
{% assign lecture_one_room = "PHYS G.42" %}

{% assign lecture_two_day = "Tuesday" %}
{% assign lecture_two_time = "10am" %}
{% assign lecture_two_room = "PHYS G.42" %}

{% assign lab_day = "Thursday" %}
{% assign lab_time = "3pm" %}
{% assign lab_room = "MVB 2.11/1.15" %}

## :date: Week by Week

Links to the lecture synopsis, problem sheets and their solutions will appear here as the unit progresses.

<table class="schedule">
  <thead>
    <tr> 
      <th style="text-align:center">University<br>Week</th>
      <th style="text-align:center">{{ lecture_one_day }} Lecture <br><span style="font-weight:normal;font-style:italic">({{ lecture_one_room }}, {{lecture_one_time}})</span></th>
      <th style="text-align:center">{{ lecture_two_day }} Lecture<br><span style="font-weight:normal;font-style:italic">({{ lecture_two_room }}, {{ lecture_two_time }})</span></th>
      <th style="text-align:center">{{ lab_day }} Lab<br><span style="font-weight:normal;font-style:italic">({{ lab_room }}, {{ lab_time }})</span></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="5" style="text-align:center">Welcome Week</td>
    </tr>
{% for calendar_week in (1..12) %}
  {% if calendar_week <= 6 %}
    {% assign logical_week = calendar_week %}
  {% else %}
    {% assign logical_week = calendar_week | minus: 1 %}
  {% endif %}
  {% if calendar_week == 6 %}
    {% assign qns_path = "/questions/rev6.pdf" %}
    {% assign ans_path = "/answers/rev6.pdf" %}
    {% assign qns = site.static_files | where: "path", qns_path | first %}
    {% assign ans = site.static_files | where: "path", ans_path | first %}
    <tr>
      <td colspan="5" style="text-align:center">
        Revision Week 
          {% if qns %}
            (Syntax Revision <a href="{{ qns_path | remove_first: "/" }}" target="_blank">qns</a>
              {% if ans %}
                / <a href="{{ ans_path | remove_first: "/" }}" target="_blank">ans</a>
              {% endif %}
            )
          {% endif %}
      </td>
    </tr>
  {% elsif calendar_week == 12 %}
    {% assign qns_path = "/questions/rev12.pdf" %}
    {% assign ans_path = "/answers/rev12.pdf" %}
    {% assign qns = site.static_files | where: "path", qns_path | first %}
    {% assign ans = site.static_files | where: "path", ans_path | first %}
    <tr>
      <td colspan="5" style="text-align:center">
        Revision Week 
          {% if qns %}
            (Semantics and Computability Revision <a href="{{ qns_path | remove_first: "/" }}" target="_blank">qns</a>
              {% if ans %}
                / <a href="{{ ans_path | remove_first: "/" }}" target="_blank">ans</a>
              {% endif %}
            )
          {% endif %}
      </td>
    </tr>
  {% else %}
    <tr> 
    {% assign lec_one_idx = logical_week | minus:1 | times:2 %}
      <!-- University week number -->
      <td style="text-align:center">Week {{ calendar_week }}</td>
      <!-- Lecture 1 -->
      <td style="text-align:center"> 
      {% if site.data.lectures[lec_one_idx] %}
        {% assign lec = site.data.lectures[lec_one_idx] %}
        {% if lec.replay %}
          <a href="{{ lec.replay }}" target="_blank">{{ lec.title }}</a>
        {% else %}
          {{ lec.title }}
        {% endif %}
      {% endif %}
      </td>  
      <!-- Lecture 2 -->
      <td style="text-align:center">
    {% assign lec_two_idx = logical_week | minus:1 | times:2 | plus:1 %}
    {% if site.data.lectures[lec_two_idx] %}
        {% assign lec = site.data.lectures[lec_two_idx] %}
        {% if lec.replay %}
          <a href="{{ lec.replay }}" target="_blank">{{ lec.title }}</a>
        {% else %}
          {{ lec.title }}
        {% endif %}
      {% endif %}
      </td>
      <!-- Lab -->
      <td style="text-align:center">
    {% capture qns_name %}/questions/sheet{{ calendar_week }}.pdf{% endcapture %}
    {% capture ans_name %}/answers/sheet{{ calendar_week }}.pdf{% endcapture %}
    {% capture lab_qns_name %}/questions/lab{{ calendar_week }}.pdf{% endcapture %}
    {% capture lab_ans_name %}/answers/lab{{ calendar_week }}.pdf{% endcapture %}
    {% assign qns = false %}
    {% assign ans = false %}
    {% assign lab_qns = false %}
    {% assign lab_ans = false %}
    {% for static_file in site.static_files %}
      {% if static_file.path == qns_name %}
        {% assign qns = true %}
      {% endif %}
      {% if static_file.path == ans_name %}
        {% assign ans = true %}
      {% endif %}
      {% if static_file.path == lab_qns_name %}
        {% assign lab_qns = true %}
      {% endif %}
      {% if static_file.path == lab_ans_name %}
        {% assign lab_ans = true %}
      {% endif %}
    {% endfor %}
    {% if qns %}
        Theory: <a href="{{ qns_name | remove_first: "/" }}" target="_blank">qns</a>  
    {% endif  %}
    {% if ans %}
        / <a href="{{ ans_name | remove_first: "/" }}" target="_blank">ans</a>  
    {% endif %}
    <br/>
    {% if lab_qns %}
        Practice: <a href="{{ lab_qns_name | remove_first: "/" }}" target="_blank">lab</a>
    {% endif %}
    {% if lab_ans %}
        / <a href="{{ lab_ans_name | remove_first: "/" }}" target="_blank">lab</a>
    {% endif %}
      </td>
    </tr>
  {% endif %}
{% endfor %}
  </tbody>
</table>
