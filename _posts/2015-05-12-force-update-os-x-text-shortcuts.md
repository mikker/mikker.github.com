---
title: Force-update text shortcuts in OS X Yosemite
layout: post
tags:
  - yosemite
  - osx
---
My text shortcuts never synced when I recently set up my new Macbook and so I was left inserting _all my emojis_ using `ctrl+space` and using my mouse like you would if you weren't communicating primarily by thumbs-up emoji like me 👍🏼

So, to force a refresh:

1. Turn off **iCloud Drive**
1. `rm -rf ~Library/Mobile Documents/com~apple~TextInput`
1. Turn **iCloud Drive** back on

