---
title: "Using Preact in production with Rails' Webpacker"
layout: post
tags:
  - rails
  - react
  - preact
  - webpacker
  - webpack
---
[Preact][] is like React but smaller in size. I think? Anyway, so I've been told. It's also different in some ways and I'm not entirely sure how, but they have this thing called [`preact-compat`](https://github.com/developit/preact-compat) that'll plug right into your Webpack config and use Preact instead of React which seems like free filesize savings. Right on!

First:

```sh
$ yarn add preact-compat webpack-merge
````

In `config/webpack/production.js`:

```javascript
const environment = require('./environment')
const merge = require('webpack-merge')

module.exports = merge(environment.toWebpackConfig(), {
  resolve: {
    alias: {
      'react': 'preact-compat',
      'react-dom': 'preact-compat'
    }
  }
})
```

I saved 2 whole megabytes with just this change. No, of course that's not true.

[Preact]: https://preactjs.com
