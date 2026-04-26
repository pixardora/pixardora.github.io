---
layout: default
title: Dora's Website
heatmap_start: "2025-04-27"
---


I am a postdoctoral fellow at the Oxford School of Global and Area Studies. I study history of medicine, science, and the environment. I enjoy doing fieldwork in western China, mainly the Upper Min River region in Sichuan province. I am currently writing a monograph on the life stories of medicine gatherers of Chinese medicines.

![The Min River](assets/images/valley.jpeg)

_A glimpse of the Min River Valley, Sichuan, China. Photo taken by me during fieldwork, 2019._

{% assign start_ts = page.heatmap_start | date: '%s' | plus: 0 %}
{% assign today_ts = site.time | date: '%s' | plus: 0 %}
{% assign total_seconds = today_ts | minus: start_ts %}
{% assign total_weeks = total_seconds | divided_by: 604800 %}

{% assign total_words = 0 %}
{% assign writing_days = 0 %}
{% for entry in site.data.writing %}
  {% assign val = entry[1] | plus: 0 %}
  {% assign total_words = total_words | plus: val %}
  {% if val > 0 %}{% assign writing_days = writing_days | plus: 1 %}{% endif %}
{% endfor %}
{% assign total_k = total_words | divided_by: 1000 %}

<p class="heatmap-note">Writing log · {{ page.heatmap_start | date: "%b %Y" }}–{{ site.time | date: "%b %Y" }} · {{ total_k }}k words · {{ writing_days }} writing days</p>

<div class="heatmap-wrapper">
  <table class="heatmap-table" aria-label="Writing heatmap">

    <!-- Month label row -->
    <tr>
      {% assign prev_month = "" %}
      {% for week in (0..total_weeks) %}
        {% assign week_offset = 604800 | times: week %}
        {% assign col_ts = start_ts | plus: week_offset %}
        {% assign col_month = col_ts | date: '%b %Y' %}
        {% if col_month != prev_month %}
          <td class="month-cell">{{ col_ts | date: '%b' }}</td>
          {% assign prev_month = col_month %}
        {% else %}
          <td class="month-cell"></td>
        {% endif %}
      {% endfor %}
      <td class="day-label"></td>
    </tr>

    <!-- Data rows (0=Sun … 6=Sat) -->
    {% for day in (0..6) %}
      <tr>
        {% for week in (0..total_weeks) %}
          {% assign week_offset = 604800 | times: week %}
          {% assign col_ts   = start_ts | plus: week_offset %}
          {% assign day_offset = 86400 | times: day %}
          {% assign cell_ts  = col_ts | plus: day_offset %}
          {% assign cell_date = cell_ts | date: '%Y-%m-%d' %}
          {% assign count = site.data.writing[cell_date] | default: 0 %}
          {% if count == 0 %}
            {% assign level = 0 %}
          {% elsif count < 500 %}
            {% assign level = 1 %}
          {% elsif count < 1000 %}
            {% assign level = 2 %}
          {% elsif count < 1800 %}
            {% assign level = 3 %}
          {% else %}
            {% assign level = 4 %}
          {% endif %}
          <td class="heatmap-cell level-{{ level }}"
              data-date="{{ cell_date }}"
              data-count="{{ count }}"
              aria-label="{{ cell_date }}: {{ count }} words"
              role="gridcell"></td>
        {% endfor %}
        <td class="day-label day-label-right">
          {% case day %}
            {% when 1 %}Mon
            {% when 4 %}Thu
          {% endcase %}
        </td>
      </tr>
    {% endfor %}

  </table>

  <div class="heatmap-legend">
    <span>Less</span>
    <span class="legend-cell level-0"></span>
    <span class="legend-cell level-1"></span>
    <span class="legend-cell level-2"></span>
    <span class="legend-cell level-3"></span>
    <span class="legend-cell level-4"></span>
    <span>More</span>
  </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const wrapper = document.querySelector('.heatmap-wrapper');
  if (wrapper) wrapper.scrollLeft = wrapper.scrollWidth;

  const tooltip = document.createElement('div');
  tooltip.className = 'tooltip';
  document.body.appendChild(tooltip);

  document.querySelectorAll('.heatmap-cell').forEach(cell => {
    cell.addEventListener('mouseenter', () => {
      const count = parseInt(cell.dataset.count, 10);
      tooltip.textContent = count > 0
        ? `${cell.dataset.date}: ${count.toLocaleString()} words`
        : `${cell.dataset.date}: no entry`;

      const rect = cell.getBoundingClientRect();
      tooltip.style.left = `${rect.left + window.scrollX + rect.width / 2}px`;
      tooltip.style.top  = `${rect.top  + window.scrollY - 34}px`;
      tooltip.classList.add('show');
    });
    cell.addEventListener('mouseleave', () => tooltip.classList.remove('show'));
  });
});
</script>
