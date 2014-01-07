---
layout: post
title: "OS X's open with support for STDIN"
tags:
 - osx
 - sh
---
OS X's `open` command is great and I use it many times a day. But several times I've stumbled through some command that outputs something only to be reminded once again that `open` doesn't support piping of input. Let's fix that.

````sh
#!/bin/sh
# Put a dash at the end of open to use stdin as arguments
# usage: echo "TextEdit" | open -a -
for last; do true; done
if [ "$last" == '-' ]; then
  arg=`cat`
  cmd=`echo $@ | sed -e "s/-$/${arg}/"`
  /usr/bin/open $cmd
else
  /usr/bin/open $@
fi
````

This add a check to open: If the last argument is a dash (-), then use STDIN as the argument to the original `open`.
