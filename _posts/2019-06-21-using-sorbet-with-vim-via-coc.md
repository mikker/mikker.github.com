---
title: Using Sorbet in (Neo)Vim via coc.nvim
layout: post
tags:
- ruby
- vim
- neovim
- sorbet
---
I for one welcome [Stripe's new typechecker for Ruby][sorbet]. As many of us learnt from Typescript, the added help from the typechecker is a great benefit with little to no cost.

To add Sorbet's hints and errors to your Neovim, install [coc.nvim][] and add this to your `:CocConfig`:

```json
"languageserver": {
  "sorbet": {
    "command": "srb",
    "args": ["tc", "--lsp", "--enable-all-experimental-lsp-features"],
    "filetypes": ["ruby"],
    "rootPatterns": ["sorbet/config"],
    "initializationOptions": {},
    "settings": {}
  }
}
```

Restart vim and load up a type checked Ruby file. What a time to be alive 🍦

[sorbet]: https://sorbet.org
[coc.nvim]: https://www.github.com/neoclide/coc.nvim
