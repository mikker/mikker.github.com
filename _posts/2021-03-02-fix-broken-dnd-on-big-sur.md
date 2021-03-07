---
published: true
layout: post
title: Fix macOS Big Sur being stuck in Do Not Disturb
date: 2021-03-02T06:00:00.000Z
categories: mac macos bigsur
---
For some reason, since upgrading to macOS Big Sur, my Do Not Disturb status seems to get stuck. However many times I turn it off it always mysteriously ends up being on again. Which I of course first realise after missing notification for a few hours, having been unwantingly extra productive. Can't have that!

Thanks to [this Reddit comment](https://www.reddit.com/r/MacOS/comments/kcvdn4/cannot_turn_off_the_do_not_disturb_on_big_sur_is/gj04n0t/?utm_source=reddit&utm_medium=web2x&context=3) the setting is set in one or more plists.

Use this one liner to set it to false:

```bash
for f in `ls ~/Library/Preferences/ByHost/com.apple.notificationcenterui.*.plist`; do defaults write $f doNotDisturb 0; done && killall NotificationCenter
```

Aaah, the overwhelming feeling of drowning in notifications is back.
