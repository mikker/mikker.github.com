---
title: Disable macOS' "Tile by dragging windows to screen edges" from the command line
layout: post
tags:
  - macos
---

```sh
defaults write com.apple.WindowManager EnableTilingByEdgeDrag -int 0;
```

To enable it again:

```sh
defaults write com.apple.WindowManager EnableTilingByEdgeDrag -int 1;
```
