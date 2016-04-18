---
layout: post
title: Cycle an array using ES2015
tags:
- javascript
---
ES2015 is fun and allows you to write more concise Javascript.

Here's how to cycle an array of values. Use it to zebra stripe a table or whatever:

```js
function cycle (...args) {
  return (i) => args[i % args.length]
}
```

And we can still make it shorter:

```js
const cycle = (...args) => (i) => args[i % args.length]
```

Use it in React like this:

```js
function Table ({ posts }) {
  const cls = cycle('odd', 'even')

  return <table>
    {posts.map((post, i) => (
      <tr key={i} className={cls(i)}>
        <td>{post.title}</td>
      </tr>
    ))}
  </table>
}
```

Compare it to regular ES5:

```js
function cycle () {
  var args = Array.prototype.slice.call(arguments)

  return function (i) {
    return args[i % args.length]
  }
}
```

Not that much longer but definitely less fun. We can have fun.
