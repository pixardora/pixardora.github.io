---
layout: post
title:  "Jekyll Notes"
date:   2024-09-22
tags: [techniques]
---

Yesterday I spent several hours (of course, with the help from an angel) trying to figure out just one thing: how to set up a minimalist personal site on Github. It was not that hard in the beginning. I already built a repository on Github which hosts my personal page. Real problem starts when I try to install `jekyll` -- a site manager? In order to install jekyll, I need to install `Ruby` first, following this [guide](https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/). Then this takes more than 2 hours... Anyways, here I finally set up a place to put my notes.

Just mark down the steps to run jekyll serve locally:
1. Open `terminal`;
2. `cd` to the local host folder;
3. run `bundle exec jekyll serve`;
4. add new fils under `_posts`;
5. copy the headers from previous post;
4. _Remember_ to `ctrl` + `c` to stop the serve!

Also, to insert date and time, just press `shift`+`command`+`i`.