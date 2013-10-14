---
layout: post
title: "Copy the URL of the current page and return - using Drafts for iOS and this bookmarklet"
tags:
 - ios
 - drafts
 - bookmarklet
 - javascript
---
When I've read something and want to link to it or send it to a friend I need to grab the URL from Safari. This consists of tapping the minimized address bar, then tap the address bar, then tap a million times to make the Copy/Paste popup show, then copy. This is not easy.

With this bookmarklet in your Favorites folder, it's as easy as tap, tap, tap and you're done.

```` js
var a=encodeURI(window.location.href);window.location='drafts://x-callback-url/create?text='+a+'&action=Copy%20to%20Clipboard&x-success='+a;
````
[Drafts](https://itunes.apple.com/en/app/drafts/id502385074?mt=8) opens up, copies the url and sends you back to Safari. 