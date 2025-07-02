---
layout: page
title: Tools
permalink: /tools/
---

Welcome to my toolbox 🧰

I love trying out different tools to help with writing and staying productive. But honestly, sometimes I just can’t find one that does exactly what I need. So with a little help from LLMs and AI, I’ve started making my own. 

p.s. my main writing tool is [Scrivener](https://www.literatureandlatte.com/scrivener/overview). I’m probably only using 10% of what it can do—but I honestly can’t write without it ❤️.


<ul>
  {% for tool in site.tools %}
    <li><a href="{{ tool.url | relative_url }}">{{ tool.title }}</a></li>
  {% endfor %}
</ul>