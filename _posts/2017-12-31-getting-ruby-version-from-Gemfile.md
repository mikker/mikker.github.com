---
title: Getting a project's Ruby version from the Gemfile
layout: post
tags:
- ruby
- sh
---
It used to be that every project had its own `.ruby_version` file. But you'd also have to specify the same version in the `Gemfile`. I say nay to `.ruby_version` and instead get it from **one** place:

```zsh
alias ruby-vers="cat Gemfile | grep '^ruby' | sed -E \"s/.*[\\\"'](.+)[\\\"']/\1/"\"
```

Now you can <code>$ chruby `ruby-vers`</code> your way to success!
