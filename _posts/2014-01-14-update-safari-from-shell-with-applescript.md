---
layout: post
title: "Update Safari from vim with AppleScript"
tags:
 - sh
 - applescript
 - vim
---
When Livereload is too much and cmd-tab'ing too little, there's always this AppleScript.

````sh
#!/bin/sh
osascript -e 'tell application "Safari"
  set _url to URL of current tab of front window
  set URL of current tab of front window to _url
end tell'
````

Put it in your PATH, chmod it +x, and map it to what you like in vim:

````vim
map ,r :call system("update_safari.sh")
````

You could even auto-run it when you save the current buffer:

````vim
autocmd BufWritePost <buffer> :call system("update_safari.sh")
````

Replace `<buffer>` with `*` to do it in every buffer.

