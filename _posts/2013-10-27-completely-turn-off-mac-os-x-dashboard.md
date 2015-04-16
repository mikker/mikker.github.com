---
layout: post
title: "Completely turn off Mac OS X Dashboard"
tags:
 - osx
---
Mavericks brings yet another version of Apple's OS X and the continued existence of Dashboard confuses everyone. Here's how to completely turn it off:

````sh
$ defaults write com.apple.dashboard mcx-disabled -boolean YES
````

Also for the hardcore &mdash; Completely turn off Desktop:

````sh
$ defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
````

The folder will still exist it just wont be your _desktop_. You might have to re-log in for it to take effect. With these two power-settings you're ready to concentrate and do your best procrastination on something better than _files_ or _widgets_ &ndash; fx Twitter.
