---
layout: post
title: "A favourite from my vimrc: Full-height and full-width splits"
tags:
 - vim
---
I use vim split windows all the time. Instead of tabbing between files I just open another split. Especially if the change is small.

Here's to of my favourties from my `vimrc`: `<c-w>S` to open a full-width, horizontal split topmost and `<c-w>V` to open a full-height, vertical split rightmost.

````vim
" Open splits at top level
map <c-w>V :botright :vertical :split<cr>
map <c-w>S :topleft :split<cr>
````

This is what it could look like:

![Vim](/images/2013-10-19 15_14_27.gif)

Pretty simple. Pretty neat.
