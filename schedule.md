---
layout: page
title: Schedule
permalink: /schedule/
hide_title: true
wide: true
---

{%- comment -%}
  One row per session, grouped by week: the Week cell spans that week's sessions.
  Rows come from _data/schedule.yml; the key under the table from
  _data/schedule_key.yml.

  Date shows the week's dates, spanning its rows, until a session in that week
  has a date of its own. From then on each row of that week shows its own date.

  A week with neither dates nor sessions is a slot still waiting to be filled in,
  so it is left out. Until at least one week has something, the page says so
  instead of printing an empty table.

  Cells that would print an em dash carry .is-empty, so the phone layout, which
  stacks each session as a block, can leave them out.

  On phones the Unit cell reads "Unit III". A Unit value that is a word, such as
  "Review", carries .t-unit--word so it prints without that prefix.
{%- endcomment -%}
{%- assign weeks = "" | split: "" -%}
{%- for w in site.data.schedule -%}
  {%- if w.sessions and w.sessions.size > 0 -%}{%- assign weeks = weeks | push: w -%}
  {%- elsif w.dates and w.dates.size > 0 -%}{%- assign weeks = weeks | push: w -%}
  {%- endif -%}
{%- endfor -%}
{%- capture dash -%}<span class="t-mono">—</span>{%- endcapture -%}

{% if weeks.size == 0 %}
<p class="narrow">The week-by-week schedule will be posted before classes begin.</p>
{% else %}
<div class="table-scroll" markdown="0">
<table class="schedule">
  <thead>
    <tr>
      <th class="t-week">Week</th>
      <th class="t-date">Date</th>
      <th class="t-session">Session</th>
      <th class="t-unit">Unit</th>
      <th class="t-topic">Topic</th>
      <th class="t-notes">Lecture Notes</th>
      <th class="t-reading">Reading</th>
      <th class="t-res">Additional Resources</th>
    </tr>
  </thead>
  <tbody>
  {%- for w in weeks %}
    {%- assign span = 1 -%}
    {%- if w.sessions and w.sessions.size > 0 -%}{%- assign span = w.sessions.size -%}{%- endif -%}
    {%- assign session_dates = false -%}
    {%- for s in w.sessions -%}
      {%- if s.date and s.date != "" -%}{%- assign session_dates = true -%}{%- endif -%}
    {%- endfor -%}
    {%- capture week_cells -%}
      <td class="t-week" rowspan="{{ span }}"><span class="t-week__num">{{ w.week }}</span></td>
      {%- unless session_dates -%}
        {%- if w.dates and w.dates.size > 0 %}
      <td class="t-date t-date--week" rowspan="{{ span }}"><ul class="bare">{% for d in w.dates %}<li>{{ d }}</li>{% endfor %}</ul></td>
        {%- else %}
      <td class="t-date t-date--week is-empty" rowspan="{{ span }}">{{ dash }}</td>
        {%- endif -%}
      {%- endunless -%}
    {%- endcapture -%}
    {%- if w.sessions and w.sessions.size > 0 -%}
    {%- for s in w.sessions -%}
    {%- capture row_class -%}{% if forloop.first %}is-week-start{% endif %}{% if forloop.last %} is-week-end{% endif %}{%- endcapture -%}
    {%- assign row_class = row_class | strip %}
    <tr{% if row_class != "" %} class="{{ row_class }}"{% endif %}>
      {%- if forloop.first %}
      {{ week_cells }}
      {%- endif -%}
      {%- if session_dates -%}
        {%- if s.date and s.date != "" %}
      <td class="t-date">{{ s.date }}</td>
        {%- else %}
      <td class="t-date is-empty">{{ dash }}</td>
        {%- endif -%}
      {%- endif %}
      <td class="t-session t-mono">{{ s.id | default: "—" }}</td>
      {%- if s.unit and s.unit != "" %}
      {%- assign unit_start = s.unit | slice: 0 %}
      <td class="t-unit{% unless unit_start == "I" or unit_start == "V" %} t-unit--word{% endunless %}">{{ s.unit }}</td>
      {%- else %}
      <td class="t-unit is-empty">{{ dash }}</td>
      {%- endif %}
      <td class="t-topic">{{ s.topic }}</td>
      {%- if s.notes and s.notes.size > 0 %}
      <td class="t-notes" data-label="Lecture Notes"><ul class="bare">{% for f in s.notes %}<li><a href="{{ f.url | relative_url }}">{{ f.name }}</a></li>{% endfor %}</ul></td>
      {%- else %}
      <td class="t-notes is-empty">{{ dash }}</td>
      {%- endif -%}
      {%- if s.reading and s.reading != "" %}
      <td class="t-reading" data-label="Reading">{{ s.reading }}</td>
      {%- else %}
      <td class="t-reading is-empty">{{ dash }}</td>
      {%- endif -%}
      {%- if s.resources and s.resources.size > 0 %}
      <td class="t-res" data-label="Additional Resources"><ul class="bare">{% for f in s.resources %}<li><a href="{{ f.url | relative_url }}">{{ f.name }}</a></li>{% endfor %}</ul></td>
      {%- else %}
      <td class="t-res is-empty">{{ dash }}</td>
      {%- endif %}
    </tr>
    {%- endfor -%}
    {%- else %}
    <tr class="is-week-start is-week-end">
      {{ week_cells }}
      <td class="t-session is-empty">{{ dash }}</td>
      <td class="t-unit is-empty">{{ dash }}</td>
      <td class="t-topic is-empty">{{ dash }}</td>
      <td class="t-notes is-empty">{{ dash }}</td>
      <td class="t-reading is-empty">{{ dash }}</td>
      <td class="t-res is-empty">{{ dash }}</td>
    </tr>
    {%- endif -%}
  {%- endfor %}
  </tbody>
</table>
</div>

<div id="key" class="schedule-key" markdown="0">
  <h2 class="section__title">Key</h2>
  <dl class="specs key">
    {%- for k in site.data.schedule_key.labels %}
    <div class="specs__row"><dt>{{ k.label }}</dt><dd>{{ k.text | markdownify | remove: "<p>" | remove: "</p>" | strip }}</dd></div>
    {%- endfor %}
  </dl>
</div>
{% endif %}
