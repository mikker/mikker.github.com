---
layout: post
title: "Rename current TMUX session to current directory's basename"
tags:
 - tmux
 - sh
---

I like having a seperate TMUX session per project I'm currently working on. So a numbered list of sessions quickly becomes unhandy.

````sh
tmux rename-session `basename $(pwd)`
````

`ctrl-b s` and we get this:

````
(0) + angular-firmafon: 1 windows (attached)
(1) + blog: 3 windows (attached)
(2) + wallboard: 2 windows
````

Bonus tip: Put `bind ^S switch-client -l` in your `.tmux.conf`. Now `ctrl-b ctrl-s` toggles between the current and last session.

