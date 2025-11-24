---
layout: default
title: Dora's Website
---


<!-- ## Just a place to locate my notebooks. -->

I am a postdoctoral fellow at the Oxford School of Global and Area Studies. I study history of medicine, science, and the environment. I enjoy doing fieldwork in western China, mainly the Upper Min River region in Sichuan province. I am currently writing a monograph on the life stories of medicine gatherers of Chinese medicines.

![The Min River](assets/images/valley.jpeg)

_A glimpse of the Min River Valley, Sichuan, China. Photo taken by me during fieldwork, 2019._

<!-- **Stop worrying about the style, focus on your writing.**

- Looks great on *any* device
- Tiny, optimized, and awesome pages
- No trackers, ads, or scripts, *did I mention minimal already?*
- Auto light and dark themes
- Tag support, to filter blog pages
- Quick, *15 minute* setup
- Gallery view for your images
- Code highlighting -->


<!-- Contribution Heatmap -->
<head>
  <link rel="stylesheet" href="/assets/style.css">
  <style>
    .contribution-chart {
      display: inline-block;
      padding: 8px;
      background: #f5f5f5;
      border-radius: 8px;
      overflow-x: auto;
      max-width: 95%;
    }
    .contribution-table {
      border-spacing: 2px;
      background: transparent;
    }
    .contribution-day {
      width: 8px;
      height: 8px;
      border-radius: 2px;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    .contribution-day:hover {
      transform: scale(1.3);
      box-shadow: 0 0 6px rgba(0,0,0,0.4);
    }
    .contribution-level-0 { background: #ebedf0; } /* Light Gray */
    .contribution-level-1 { background: #fff9c4; } /* Light Yellow */
    .contribution-level-2 { background: #fff176; } /* Yellow */
    .contribution-level-3 { background: #ffb300; } /* Dark Yellow */
    .contribution-level-4 { background: #ff8f00; } /* Deep Orange */
    .tooltip {
        position: absolute;
        background: red;
        color: #fff; /* 修改文本颜色为白色 */
        padding: 8px 12px;
        border-radius: 4px;
        font-family: sans-serif;
        font-size: 14px;
        pointer-events: none;
        opacity: 1;
        transition: opacity 0.3s ease;
        z-index: 9999;
        /* white-space: nowrap; */
        /* 添加visibility控制确保隐藏 */
        visibility: visible;
        transform: translateX(-50%);
    }
    .tooltip.show {
        opacity: 1;
        visibility: visible;
    }
    .day-label {
      font-size: 10px;
      color: #333;
      text-align: right;
      padding-right: 5px;
    }
    .legend {
      margin-top: 10px;
      font-size: 10px;
      display: flex;
      align-items: center;
      gap: 5px;
      justify-content: flex-end;
    }
    .legend-item {
      width: 12px;
      height: 12px;
      display: inline-block;
    }
  </style>
</head>

<head>
    Writing log since May 2025. Late September: finished the first draft — then drifted “at sea” for a while. Lots of new project ideas surfaced, but none felt right to land on. Mid-October: serious (sort of) writing restarted amid a flurry of applications.
</head>

<div class="contribution-chart">
  <table class="contribution-table" aria-label="Writing contribution heatmap">
    {% assign today = site.time | date: '%s' %}
    {% assign one_year_ago = today | minus: 31536000 %}
    
    <!-- Pre-calculate constants -->
    {% assign weeks = 52 %}
    {% assign days = 7 %}

    <!-- 计算 one_year_ago 的星期几（0=Sun, 1=Mon, ...） -->
    {% assign one_year_ago_date = one_year_ago | date: '%w' | plus: 0 %}
    <!-- 调整到最近的周日（减去星期几的天数） -->
    {% assign days_to_sunday = one_year_ago_date | times: 86400 %}
    {% assign one_year_ago_sunday = one_year_ago | minus: days_to_sunday %}

    <!-- Debugging: Check if data is loaded -->
    {% if site.data.writing %}
      <!-- Rows represent days of the week (0=Sunday, 6=Saturday) -->
      {% for day in (0..6) %}
        <tr>
          <!-- Add day labels -->
          <td class="day-label">
            {% case day %}
              {% when 0 %}Sun
              {% when 1 %}Mon
              {% when 2 %}Tue
              {% when 3 %}Wed
              {% when 4 %}Thu
              {% when 5 %}Fri
              {% when 6 %}Sat
            {% endcase %}
          </td>
          {% for week in (0..weeks) %}
            <!-- 修复周起始和日偏移的计算顺序 -->
            {% assign week_offset = 604800 | times: week %}
            {% assign week_start = one_year_ago_sunday | plus: week_offset %}
            {% assign day_offset = 86400 | times: day %}
            {% assign current_date_ts = week_start | plus: day_offset %}
            {% assign current_date = current_date_ts | date: '%Y-%m-%d' %}
            {% assign count = site.data.writing[current_date] | default: 0 %}
            {% assign level = count | divided_by: 100 | at_least: 0 | at_most: 4 %}

            <td class="contribution-day contribution-level-{{ level }}"
                data-date="{{ current_date }}"
                data-count="{{ count }}"
                aria-label="{{ current_date }}: {{ count }} words"
                role="gridcell"
                onmouseover="this.title='{{ current_date }}: {{ count }} words'"></td>

          {% endfor %}
        </tr>
      {% endfor %}
    {% else %}
      <tr><td colspan="53" style="color: red;">Error: writing.yml data not loaded. Please ensure _data/writing.yml exists.</td></tr>
    {% endif %}
  </table>

  <!-- Legend for contribution levels -->
  <div class="legend">
    <span>Less</span>
    <span class="legend-item contribution-level-0"></span>
    <span class="legend-item contribution-level-1"></span>
    <span class="legend-item contribution-level-2"></span>
    <span class="legend-item contribution-level-3"></span>
    <span class="legend-item contribution-level-4"></span>
    <span>More</span>
  </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const cells = document.querySelectorAll('.contribution-table .contribution-day');
  const tooltip = document.createElement('div');
  tooltip.className = 'tooltip';
  document.body.appendChild(tooltip);

  cells.forEach(cell => {
    cell.addEventListener('mouseenter', (e) => {
      const date = cell.dataset.date || 'No date';
      const count = cell.dataset.count || '0';
      tooltip.textContent = `${date}: ${count} words`;

      // Calculate position
      const rect = cell.getBoundingClientRect();
      const scrollX = window.scrollX || window.pageXOffset;
      const scrollY = window.scrollY || window.pageYOffset;

      // Center tooltip above the cell
      const left = rect.left + scrollX + (rect.width / 2);
      const top = rect.top + scrollY - 30; // Adjust to position above cell

      tooltip.style.left = `${left}px`;
      tooltip.style.top = `${top}px`;

      // Show tooltip
      tooltip.classList.add('show');
    });

    cell.addEventListener('mouseleave', () => {
      // Hide tooltip
      tooltip.classList.remove('show');
    });
  });
});
</script>