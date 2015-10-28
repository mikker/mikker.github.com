---
layout: post
title: Recreating DuckDuckGo's "I'm Feeling Lucky" bang method on Google
tags:
  - javascript
  - dotjs
---
DuckDuckGo has a [slew of bang methods]() that let you do all sorts of things with your search terms. One of them is just the _bang_ (!) that'll automatically go to the topmost result. Like Google's "I'm Feeling Lucky".

How can we do that on Google aswell? Using [dotjs]()! Long story short, dotjs let's you write javascript files that run after sites load eg. `~/.js/google.com.js` in this instance.

```javascript
(function (window, document) {
  'use strict'

  console.log('dotjs')

  // Parse the search string to an object
  var params = window.location.search
  .replace(/^\?/, '')
  .split('&')
  .reduce(function (params, kv) {
    kv = kv.split('=')
    params[kv[0]] = kv[1]
    return params
  }, {})

  // See if the ?q param contains a bang
  if (params.q.match(/\!/)) {
    // Click the topmost result
    document.querySelector('h3.r a').click()
  }
})(window, document)
```
