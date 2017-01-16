---
layout: post
title: Sprinkling React
tags:
- react
- js
---
A beautiful thing about jQuery was how you could sprinkle it on top of an already working site. With all of today's SPAs and what nots this seems like a distant past. We've all drunk the Kool Aid and the benefits of React (and the like) are just too huge to ever look back.

Still, there's no reason we can't still sprinkle instead SPA-ing. That was a horrible sentence. Let's move on.

Here's what I do on [10er.dk](https://10er.dk).

The api is basically as follows. You have a div with some `data-` props that we'll catch later and switch in the magic.

```html
<article>
  <div
    data-react='NumberWang'
    data-props='{"numbers":[1,2,3,5,68]}'
  ></div>
</article>
```

Ok, so that means _look up the component called `NumberWang`, render it with these props and throw it back in here_.

Here's `sprinkleComponents.js`. It uses Webpack and its [`require.context`][context]:

```js
import React from 'react'
import { render } from 'react-dom'

let context = require.context('./components', true, /\.(\/index)?js$/)

export default function sprinkleComponents () {
  const nodes = document.querySelectorAll('[data-react]')

  for (const node of nodes) {
    const regexp = new RegExp(node.dataset.react + '(/index)?\\.js')
    const match = context.keys().find(path => path.match(regexp))
    const Comp = context(match).default
    const props = node.dataset.reactProps
      ? JSON.parse(node.dataset.reactProps)
      : {}
    props.children = node.innerHTML

    render(<Comp {...props} />, node)
  }
}

// We can even get hot module reload (so dx am I right?)
if (module.hot) {
  module.hot.accept(context.id, (upd) => {
    context = require.context('./components', true, /\.(\/index)?js$/)
    sprinkleComponents()
  })
}
```

This expects to find the component `NumberWang` at either `./components/NumberWang.js` or `./components/NumberWang/index.js` relative to `sprinkleComponents.js`.

Let's make it – `./components/NumberWang.js`:

```js
import React from 'react'

export default ({ numbers }) =>
  <div>{numbers[Math.floor(Math.random() * numbers.length)]}</div>
```

And finally in your `main.js` or `index.js` or `client.js` or whatever:

```js
document.addEventListener('DOMContentLoaded', () => {
  sprinkleComponents()
})
```

This is the technique that I use to selectively apply some js sauce on [10er.dk](https://10er.dk), a mostly server rendered Rails app.

[context]: https://webpack.github.io/docs/context.html
