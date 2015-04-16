---
layout: post
title: "Install Qt on OS X Yosemite (Beta 3)"
tags:
  - os x
  - homebrew
  - qt
---
To install [Qt](http://qt-project.org/) on Yosemite (Beta 3 right now) using [homebrew](http://brew.sh/), just act as if you're on Mavericks and it will work fine:

Edit `/usr/local/Library/Homebrew/os/mac/version.rb`:

```ruby
def to_sym
  return :mavericks # <-- stupid hack.
  SYMBOLS.invert.fetch(@version) { :dunno }
end
```

Then when you're done; clean up after yourself!
