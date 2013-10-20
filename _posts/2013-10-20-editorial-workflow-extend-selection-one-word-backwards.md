---
layout: post
title: "Editorial workflow: extend selection one word backwards"
tags:
 - editorial
 - editorial-workflow
---
[Editorial][] is my favourite iPad editor. Here's [a workflow][workflow] that I made. It extends the current selection one word backwards.

[![Screenshot](/images/2013-10-20 at 20.23.png)][workflow]

Because of how Editorial workflows work I had to do a bit of hackery to make it work. The idea is to find the starting position of the word just before the current selection. My first version looped through every single word up till the selection but that was unusably slow on large documents. So this version only loops through every word in the 50 letters leading up to the selection. A hack, but it works really well.

[workflow]: http://www.editorial-workflows.com/workflow/5245546637819904/EPQ8ocEnB0w "Extend backwards"
[Editorial]: http://omz-software.com/editorial/ "Editorial for iPad"
