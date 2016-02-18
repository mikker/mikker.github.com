---
title: Updated Elixir Phoenix 1.1.4 with Webpack and React Hot Module Reload
layout: post
tags:
  - phoenix
  - elixir
  - webpack
  - react
---
**Updated instructions for Phoenix 1.1.4 and custom Webpack dev server**

Generate your new app without brunch:

```sh
$ mix phoenix.new my_app --no-brunch
$ cd my_app
```

This is file that we want to be able to compile, `web/static/js/index.js`:

```js
// Phoenix' dependencies
import '../../../deps/phoenix/priv/static/phoenix'
import '../../../deps/phoenix_html/priv/static/phoenix_html'

// Shiny new, hot React component
import React, { Component } from 'react'
import { render } from 'react-dom'

class Root extends Component {
  render () {
    return <h1>omg so hot</h1>
  }
}

render(<Root />, document.getElementById('root'))
```

Install the required js dependencies using `npm`:

```sh
$ echo '{"private": true}' > package.json
$ npm install --save babel-core babel-polyfill babel-loader babel-preset-es2015 babel-preset-react react react-dom webpack
$ npm install --save-dev webpack-dev-middleware webpack-hot-middleware express cors babel-preset-react-hmre babel-preset-stage-0 babel-preset-es2015
```

A lot of stuff, right?

We need a webpack config. Here's `webpack.config.js`:

```javascript
var path = require('path')
var webpack = require('webpack')
var publicPath = 'http://localhost:4001/'

var env = process.env.MIX_ENV || 'dev'
var prod = env === 'prod'

var entry = './web/static/js/index.js'
var hot = 'webpack-hot-middleware/client?path=' +
  publicPath + '__webpack_hmr'

var plugins = [
  new webpack.optimize.OccurrenceOrderPlugin(),
  new webpack.NoErrorsPlugin(),
  new webpack.DefinePlugin({
    __PROD: prod,
    __DEV: env === 'dev'
  })
]

if (env === 'dev') {
  plugins.push(new webpack.HotModuleReplacementPlugin())
}

module.exports = {
  devtool: prod ? null : 'cheap-module-eval-source-map',
  entry: prod ? entry : [hot, entry],
  output: {
    path: path.resolve(__dirname) + '/priv/static/js',
    filename: 'index.bundle.js',
    publicPath: publicPath
  },
  plugins: plugins,
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        loaders: ['babel'],
        exclude: path.resolve(__dirname, 'node_modules')
      }
    ]
  }
}
```

And we need a node server for development, `webpack.dev.js`:

```javascript
#!/usr/bin/env node
var express = require('express')
var webpack = require('webpack')
var config = require('./webpack.config')

var compiler = webpack(config)
var app = express()
app.use(require('cors')())

app.use(require('webpack-dev-middleware')(compiler, {
  noInfo: true,
  publicPath: config.output.publicPath
}))

app.use(require('webpack-hot-middleware')(compiler, {
  log: console.log
}))

app.listen(4001, 'localhost', function (err) {
  if (err) return console.error(err)
  console.log('dev server running on localhost:4001')
})

// Exit on end of STDIN
process.stdin.resume()
process.stdin.on('end', function () {
  process.exit(0)
})
```

Allow it to be executed:

```sh
$ chmod +x webpack.dev.js
```

Babel 6 needs a `.babelrc` so let's add it:

```json
{
  "presets": ["es2015", "react", "stage-0"],
  "env": {
    "development": {
      "presets": ["react-hmre"]
    }
  }
}
```

Make your app run that script as a watcher in `dev`. `config/dev.exs`:

```elixir
config :speaker, MyApp.Endpoint,
  http: [port: 4000],
  debug_errors: true,
  code_reloader: true,
  check_origin: false,
  watchers: [{Path.expand("webpack.dev.js"), []}]

# ...

config :speaker, MyApp.Endpoint,
  live_reload: [
    patterns: [
      # ~r{priv/static/.*(js|css|png|jpeg|jpg|gif|svg)$},
      ~r{priv/gettext/.*(po)$},
      ~r{web/views/.*(ex)$},
      ~r{web/templates/.*(eex)$}
    ]
  ]
```

And finally add this to the bottom of `web/templates/layout/app.html.eex`:

```erb
<%= if Mix.env == :dev do %>
  <script src='http://localhost:4001/index.bundle.js'></script>
<% else %>
  <script src="<%= static_path(@conn, "/js/index.bundle.js") %>"></script>
<% end %>
```

And `web/templates/page/index.html.eex` is just:

```html
<div id='root'></div>
```

### Building for production

Add `lib/mix/tasks/digest.ex`:

```elixir
defmodule Mix.Tasks.MyApp.Digest do
  use Mix.Task

  def run(args) do
    Mix.Shell.IO.cmd "NODE_ENV=production ./node_modules/webpack/bin/webpack.js -p"
    :ok = Mix.Tasks.Phoenix.Digest.run(args)
  end
end
```

And in `mix.exs`:

```elixir
defmodule MyApp.Mixfile do
  # ...
  def project do
    [ # ...
      aliases: ["phoenix.digest": "my_app.digest"]]
  end
  # ...
end
```

