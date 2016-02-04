---
layout: post
title: "One-shot command to create a Github pull-request and open it in your browser"
tags:
 - git
 - sh
---
At [Firmafon][] we almost always concentrate updates into [Github][]'s pull reqeusts. This provides a nice overview of the changes and an obvious place to discuss them. Here's two scripts that I use to make this process even easier.

The first one's called `last_commit_message`:

```
#!/bin/sh
git --no-pager log -1 --pretty=%B | sed -e "s/^ *//g" -e "s/ *$//g" | tr -d "\n"
```

This outputs the last commit's message, inline without anything else but the message.

The other is called `prl`:

````sh
#!/bin/sh -x
set -e

last_msg="`last_commit_message`"
git push -u
url=`hub pull-request "$last_msg"`
open "$url"
````

This get's the last commit's message for use as the pull request's title. Then it pushes to a remote branch.

Then using Github's [hub][] command (`brew install hub`) it creates a pull request from the current HEAD, catches the returned url and `open`'s it.

[Firmafon]: https://firmafon.dk
[Github]: https://help.github.com/articles/using-pull-requests
[hub]: https://github.com/github/hub
