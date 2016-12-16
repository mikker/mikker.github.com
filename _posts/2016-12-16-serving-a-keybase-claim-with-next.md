---
layout: post
title: Serving a Keybase.io claim with Next
tags:
- next.js
- keybase.io
- javascript
---
I recently moved my personal website to [next.js][] because why not. The people at [▲ Zeit][zeit] are on a roll and I want in on it.

[Keybase][] quickly started complaining about me removing my _claim_. You're supposed to host a file at `/keybase.txt` with specific content to claim a domain is truly yours. So how do we do that with Next?

Here's `pages/keybase.txt.js`:

```javascript
import React, { Component } from 'react'

export default class KeybaseTxt extends Component {
  static getInitialProps ({ res }) {
    res.end(claim)
  }
  
  render () {
    return <div>We'll never get this far</div>
  }
}

const claim = `=====...long long claim thing...`
```

`getInitialProps` is Next's way of fetching data before render so it works both on the server and the client. During server render it's passed `req` and `res` from node and apparently we can just end it right there. So dumb it's smart.

[next.js]: https://github.com/zeit/next.js
[zeit]: https://zeit.co
[Keybase]: https://keybase.io
