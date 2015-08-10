---
title: Use webpack-dev-server and react-hot-loader with Phoenix
layout: post
tags:
  - elixir
  - phoenix
  - react
  - webpack
---
[Phoenix][] has built-in livereload and it works right out of the box. But if you've ever had the joy of working with [React][] and [react-hot-loader][] you know you need to have that with you anywhere.

Phoenix uses [Brunch][] to build it's assets so we'll have to pull that out and jam in a [webpack-dev-server][] wherever it was. Luckily that's quite easy.

**The completed example is [available on Github][]**.

First, let's create a new app:

```sh
$ mix phoenix.new my_app
$ cd my_app
```

We could've generated the app without Brunch but let's keep it in, to see what we're actually substituting.
Let's start by pulling out Brunch and it's dependencies and then add our new ones:

```sh
$ npm uninstall --save babel-brunch brunch clean-css-brunch css-brunch javascript-brunch sass-brunch uglify-js-brunch
$ npm install --save babel-loader react react-hot-loader webpack webpack-dev-server
```

Now, webpack needs to be told what to build and how, so let's make a config file. Here's `webpack.config.js`:

```javascript
var path = require('path')
var webpack = require('webpack')

var env = process.env.MIX_ENV || 'dev'
var prod = env === 'prod'

var entry = './web/static/js/bundle.js'
var plugins = [new webpack.NoErrorsPlugin()]
var loaders = ['babel']
var publicPath = 'http://localhost:4001/'

if (prod) {
  plugins.push(new webpack.optimize.UglifyJsPlugin())
} else {
  plugins.push(new webpack.HotModuleReplacementPlugin())
  loaders.unshift('react-hot')
}

module.exports = {
  devtool: prod ? null : 'eval-sourcemaps',
  entry: prod ? entry : [
    'webpack-dev-server/client?' + publicPath,
    'webpack/hot/only-dev-server',
    entry
  ],
  output: {
    path: path.join(__dirname, './priv/static/js'),
    filename: 'bundle.js',
    publicPath: publicPath
  },
  plugins: plugins,
  module: {
    loaders: [
      { test: /\.jsx?/, loaders: loaders, exclude: /node_modules/ }
    ]
  }
}
```

I will not go into too much detail but notice the address `http://localhost:4001`. That's where `webpack-dev-server` will be running from when we're developing.

Next, let's set up the dev server in `webpack.devserver.js`:

```js
#!/usr/bin/env node
var webpack = require('webpack')
var WebpackDevServer = require('webpack-dev-server')
var config = require('./webpack.config')

new WebpackDevServer(webpack(config), {
  contentBase: 'http://localhost:4001',
  publicPath: config.output.publicPath,
  hot: true
}).listen(4001, '0.0.0.0', function (err, result) {
  if (err) console.error(err)
  console.log('webpack-dev-server running on port 4001')
})

// Exit on end of STDIN
process.stdin.resume()
process.stdin.on('end', function () {
  process.exit(0)
})
```

That last last bit is there to make the process shut down properly when Phoenix shuts down. (A big thank you to [josevalim][] for guiding me in figuring that out, for being such a seemingly nice guy and of course for making Elixir!)

Don't forget to make it executable:

```sh
$ chmod +x webpack.devserver.js
```

We need to tell Phoenix to run this instead of Brunch, so open up `config/dev.exs` and the change `watchers:` line to include our dev server instead:

```elixir
config :my_app, MyApp.Endpoint,
  http: [port: 4000],
  debug_errors: true,
  code_reloader: true,
  cache_static_lookup: false,
  watchers: [{Path.expand("webpack.devserver.js"), []}]
```

And let's just cut Phoenix some slack and tell it not to watch the assets:

```elixir
config :my_app, MyApp.Endpoint,
  live_reload: [
    patterns: [
      # ~r{priv/static/.*(js|css|png|jpeg|jpg|gif)$},
      ~r{web/views/.*(ex)$},
      ~r{web/templates/.*(eex)$}
    ]
  ]
```

OK, let's make our entry file, `web/static/js/bundle.js`:

```js
import React from 'react'
import App from './App'

React.render(<App />, document.getElementById('root'))
```

This renders our App (that we haven't made yet) to an element with the id `root` (that we also haven't made yet). Splitting your React components into separate files let's react-hot-loader reload them independently from each other.

Here's a simple `web/static/js/App.js`:

```js
import React, { Component } from 'react'

export default class App extends Component {
  render () {
    return (
      <div>
        <h1>This app is hot!</h1>
      </div>
    )
  }
}
```

So far, so good. Now we just need some html document to render it in.

Edit `web/templates/layout/app.html.eex`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">
    <title>Hello Phoenix!</title>
  </head>
  <body>
    <%= @inner %>
    <%= if Mix.env == :dev do %>
      <script src='http://localhost:4001/bundle.js'></script>
    <% else %>
      <script src="<%= static_path(@conn, "/js/bundle.js") %>"></script>
    <% end %>
  </body>
</html>
```

Remember that our dev server is running on `:4001`. We want to use it's `bundle.js` in dev and a built one in production.

The only thing left is the element with id `root`. Let's put it in `web/templates/pages/index.html.eex`:

```html
<div id='root'></div>
```

And we're done! Now go open `http://localhost:4000`, edit `App.js` and behold the magic of our hot reloading gods!

### <a href="#production" name='production'>Addendum: Production</a>

Phoenix compiles all it's assets with the task `phoenix.digest` which you're supposed to run before deploying. We can just remember to run `webpack` beforehand &mdash; or we can make our own digest task.

Here's `lib/mix/tasks/digest.ex`:

```elixir
defmodule Mix.Tasks.MyApp.Digest do
  use Mix.Task

  def run(args) do
    Mix.Shell.IO.cmd "./node_modules/webpack/bin/webpack.js"
    :ok = Mix.Tasks.Phoenix.Digest.run(args)
  end
end
```

Let's be fancy and override the original task so new developers or deployment scripts don't need to know about our special setup. Open `mix.exs` and alias the original to our new task:

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

Try `mix phoenix.digest` and see that webpack runs first.

[Phoenix]: http://www.phoenixframework.org
[React]: https://github.com/facebook/react
[react-hot-loader]: https://gaearon.github.io/react-hot-loader/
[Brunch]: http://brunch.io
[josevalim]: http://twitter.com/josevalim
[webpack-dev-server]: http://webpack.github.io/docs/webpack-dev-server.html
[available on Github]: http://github.com/mikker/phoenix-react-hot-loader "mikker/phoenix-react-hot-loader"
