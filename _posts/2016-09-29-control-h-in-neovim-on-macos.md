---
layout: post
title: Control-h in NeoVim on macOS
date: 2016-09-29 21:04
tags:
  - neovim
  - vim
  - macos
  - os x
---
[Apparently](https://github.com/neovim/neovim/issues/2048) `<c-h>` sends `<BS>` to `xterm256-screen` terminals and your binding doesn't work in Neovim.

There's a few fixes, but I like [this one](https://github.com/neovim/neovim/issues/2048#issuecomment-98307896) that does it in iTerm:

1. Edit -> Preferences -> Keys
1. Press `+`
1. Press `Ctrl+h` as _Keyboard Shortcut_
1. Choose _Send Escape Sequence_ as _Action_
1. Type `[104;5u` for _Esc+_

