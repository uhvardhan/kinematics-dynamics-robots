---
layout: page
title: Schedule
permalink: /schedule/
hide_title: true
wide: true
---

{%- comment -%}
  One row per session, grouped by week: the week cell spans that week's sessions.
  Rows come from _data/schedule.yml; the key under the table from
  _data/schedule_key.yml.

  Date and Files appear only once some session has a date or a file, so the
  table doesn't carry a column of dashes while the timetable is pending.

  A week with neither dates nor sessions is a slot still waiting to be filled in,
  so it is left out. Until at least one week has something, the page says so
  instead of printing an empty table.

  Cells that would print an em dash carry .is-empty, so the phone layout, which
  stacks each session as a block, can leave them out.
{%- endcomment -%}
{%- assign weeks = "" | split: "" -%}
{%- assign has_dates = false -%}
{%- assign has_files = false -%}
{%- for w in site.data.schedule -%}
  {%- if w.sessions and w.sessions.size > 0 -%}{%- assign weeks = weeks | push: w -%}
  {%- elsif w.dates and w.dates.size > 0 -%}{%- assign weeks = weeks | push: w -%}
  {%- endif -%}
  {%- for s in w.sessions -%}
    {%- if s.date and s.date != "" -%}{%- assign has_dates = true -%}{%- endif -%}
    {%- if s.files and s.files.size > 0 -%}{%- assign has_files = true -%}{%- endif -%}
  {%- endfor -%}
{%- endfor -%}
{%- capture dash -%}<span class="t-mono">—</span>{%- endcapture -%}

{% if weeks.size == 0 %}
<p class="narrow">The week-by-week schedule will be posted before classes begin.</p>
{% else %}
<p class="schedule-intro">Readings are section numbers in Niku, <em>Introduction to Robotics</em>, 3rd edition. Session labels, resource labels and abbreviations are explained in the <a href="#key">key below the table</a>.</p>

<div class="table-scroll" markdown="0">
<table class="schedule">
  <thead>
    <tr>
      <th class="t-week">Week</th>
      {%- if has_dates %}
      <th class="t-date">Date</th>
      {%- endif %}
      <th class="t-session">Session</th>
      <th class="t-topic">Topic</th>
      <th class="t-unit">Unit</th>
      <th class="t-reading">Reading</th>
      {%- if has_files %}
      <th class="t-files">Files</th>
      {%- endif %}
      <th class="t-res">Additional resources</th>
    </tr>
  </thead>
  <tbody>
  {%- for w in weeks %}
    {%- assign span = 1 -%}
    {%- if w.sessions and w.sessions.size > 0 -%}{%- assign span = w.sessions.size -%}{%- endif -%}
    {%- capture week_cell -%}
      <td class="t-week" rowspan="{{ span }}">
        <span class="t-week__num">{{ w.week }}</span>
        {%- if w.dates and w.dates.size > 0 -%}
        <ul class="bare t-week__dates">{% for d in w.dates %}<li>{{ d }}</li>{% endfor %}</ul>
        {%- endif -%}
      </td>
    {%- endcapture -%}
    {%- if w.sessions and w.sessions.size > 0 -%}
    {%- for s in w.sessions %}
    <tr{% if forloop.last %} class="is-week-end"{% endif %}>
      {%- if forloop.first %}
      {{ week_cell }}
      {%- endif -%}
      {%- if has_dates -%}
        {%- if s.date and s.date != "" %}
      <td class="t-date t-mono">{{ s.date }}</td>
        {%- else %}
      <td class="t-date is-empty">{{ dash }}</td>
        {%- endif -%}
      {%- endif %}
      <td class="t-session t-mono">{{ s.id | default: "—" }}</td>
      <td class="t-topic">{{ s.topic }}</td>
      {%- if s.unit and s.unit != "" %}
      <td class="t-unit">{{ s.unit }}</td>
      {%- else %}
      <td class="t-unit is-empty">{{ dash }}</td>
      {%- endif -%}
      {%- if s.reading and s.reading != "" %}
      <td class="t-reading" data-label="Reading">{{ s.reading }}</td>
      {%- else %}
      <td class="t-reading is-empty">{{ dash }}</td>
      {%- endif -%}
      {%- if has_files -%}
        {%- if s.files and s.files.size > 0 %}
      <td class="t-files" data-label="Files"><ul class="bare">{% for f in s.files %}<li><a href="{{ f.url | relative_url }}">{{ f.name }}</a></li>{% endfor %}</ul></td>
        {%- else %}
      <td class="t-files is-empty">{{ dash }}</td>
        {%- endif -%}
      {%- endif -%}
      {%- if s.resources and s.resources.size > 0 %}
      <td class="t-res" data-label="Additional resources"><ul class="bare">{% for f in s.resources %}<li><a href="{{ f.url | relative_url }}">{{ f.name }}</a></li>{% endfor %}</ul></td>
      {%- else %}
      <td class="t-res is-empty">{{ dash }}</td>
      {%- endif %}
    </tr>
    {%- endfor -%}
    {%- else %}
    <tr class="is-week-end">
      {{ week_cell }}
      {%- if has_dates %}
      <td class="t-date is-empty">{{ dash }}</td>
      {%- endif %}
      <td class="t-session is-empty">{{ dash }}</td>
      <td class="t-topic is-empty">{{ dash }}</td>
      <td class="t-unit is-empty">{{ dash }}</td>
      <td class="t-reading is-empty">{{ dash }}</td>
      {%- if has_files %}
      <td class="t-files is-empty">{{ dash }}</td>
      {%- endif %}
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
  <h2 class="section__title">Abbreviations</h2>
  <dl class="specs key">
    {%- for k in site.data.schedule_key.abbreviations %}
    <div class="specs__row"><dt>{{ k.label }}</dt><dd>{{ k.text | markdownify | remove: "<p>" | remove: "</p>" | strip }}</dd></div>
    {%- endfor %}
  </dl>
</div>
{% endif %}
