---
layout: post
title: "-webkit-user-select: none; disables input and Javascript events"
tags:
  - react
  - ios
---
I could've spared two hours of googling if I had just known this.

I had this in my css file to disable text selection in my mobile-targeted _web app_:

```css
* {
  -webkit-user-select: none;
}
```

This is nice and makes what is actually a website behave more like apps do. What I didn't know was that this also disables text input on iOS.

Fast forward two hours: Either remove the `*` rule above or re-enable `user-select` for input fields:

```css
input {
  -webkit-user-select: auto;
}
```
