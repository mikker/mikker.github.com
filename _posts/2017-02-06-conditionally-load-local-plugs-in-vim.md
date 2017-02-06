---
layout: post
title: Conditionally load local plugs in vim
tags:
- vim
- vim-plug
---
If you're using vim (you should) and vim-plug for plugins (you should) and you have seom plugins locally (you might) but still want your setup to be portable (you should) then you might want to only load plugins locally if they exist (here's how).

```vim
function! s:maybeLocalPlug(args)
  let l:localPath = $HOME . "/dev/" . expand(a:args)

  if isdirectory(l:localPath)
    Plug l:localPath
  else
    Plug 'mikker/' . expand(a:args)
  endif
endfunction

call s:maybeLocalPlug('lightline-theme-pencil')
call s:maybeLocalPlug('vim-rerunner')
call s:maybeLocalPlug('vim-dimcil')
call s:maybeLocalPlug('vim-colors-paramount')
```

This checks to see if the plugin exists locally at `~/dev/plugin-name` and if it doesn't it loads from good old Github.
