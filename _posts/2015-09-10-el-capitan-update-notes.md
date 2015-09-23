---
title: El Capitan GM update notes
layout: post
tags:
  - os x
  - el capitan
---
I've been running the beta of OS X 10.11 _El Capitan_ for some months and there've been very few hiccups. So full sail ahead on the update I say.

Upgrading to the GM broke my [homebrew][] though. I think the days of using `/usr/local` might be over as El Capitan does some stuff to enforce even stricter permissions than a simple `chown` can get rid of.

So I'm moving out! Someone (forgot who or where &ndash; sorry!) mentioned in a Github issue thread how he'd been running his homebrew out of `/Users/Shared/Developer` for some months with no problems, so that's what I did:

### Installing Homebrew outside of `/usr/local`

```sh
$ git clone https://github.com/Homebrew/homebrew.git /Users/Shared/Developer
```

Now, that directory's `bin` directory isn't in your `$PATH` (like `/usr/local/bin` is automatically) so we need to add it. Open up `~/.bashrc` or `~/.zshrc` -- whatever your preference -- and add this:

```sh
export BREW_PATH=/Users/Shared/Developer
export PATH="$BREW_PATH/bin:$PATH"
```

Done. Run `brew doctor` to confirm.

### Install `taglib-ruby` gem with Homebrew outside `/usr/local`

```sh
$ brew install taglib
$ gem install taglib-ruby -- --with-tag-dir=$BREW_PATH/Cellar/taglib/1.9.1/
```

[homebrew]: http://brew.sh "Homebrew"
