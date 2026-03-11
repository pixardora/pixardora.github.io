---
layout: post
title:  "Experiment with Claude Code"
date:   2026-03-09
tags: [techniques]
---

The original motivation was to keep myself updated with the newly publications in the field of Chinese history. The initial solution over the years is to manually subscribe to certain journal's RSS and they will email you the latest articles once in a while. 

The problem of this method is: subscribing to certain journals is not enough - I may miss some publications from unnoticed journals. 

Say, I subscribed to _Twentieth-century China_, _Late Imperial China_, etc these history journals. However, as I am working in history of science but also interested in labor history, environmental history, material culture, there are must-read articles from _Technology and Culture_, _Environmental History_, _Isis_, _History of Science_, etc. I do not want to subscribe to all these journals. So, how to solve this? 

I began to wonder: instead of waiting for RSS alerts from individual journals, could I build a site that aggregates new publications across disciplines?

I talked to Claude Code about the idea. In plan mode, it outlined the key processes. The bottleneck came down to fetching new articles. I wrote a skill.md and set up /+keyword as a shortcut for triggering fetches. But every time, Claude Code would restart the fetching process from scratch — which burned through both time and tokens. I tried using natural language to talk through the problem and fix it, but couldn't get it resolved that way. 

I now realize the more efficient approach would be to look at the actual code — understand how the agent handles fetching — and fix the problem at its source.

I haven't done that yet, though.