---
title: Disable long-press for Hyper on macOS
layout: post
tags:
- macos
- hyper
---
[Hyper][] is great and even though I still cling to iTerm I sometimes try and shake things up by using Hyper for a few days.

macOS has a feature where if you press a character like _u_ and hold it, a popup menu appears that'll let you pick out a _ü_ instead. Great if you're writing German prose. Not great if you are undoing a whole day's work in vim.

So let's disable that feature only for Hyper. Put this in your term and smoke it:

```sh
defaults write co.zeit.hyper ApplePressAndHoldEnabled -bool false
```

If you want to disable it system wide then use the same command but without the `co.zeit.hyper` part.

[Hyper]: https://hyper.is
