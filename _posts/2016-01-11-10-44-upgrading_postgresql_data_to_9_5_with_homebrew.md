---
title: Upgrading PostgreSQL data from 9.4 to 9.5 with homebrew
layout: post
tags:
  - postgresql
  - homebrew
---
I got this in my `server.log`:

```
The data directory was initialized by PostgreSQL version 9.4, which is not compatible with this version 9.5.0.
```

So we have to upgrade our data. Luckily there's [`pb_upgrade`](http://www.postgresql.org/docs/9.5/static/pgupgrade.html).

First we need to have a version of 9.4 installed.

```sh
# move 9.5 out of the way
# - don't worry - your data will not be touched
$ brew uninstall postgres

# the last version of the postgres bottle with 9.4
$ brew install https://github.com/Homebrew/homebrew/raw/f8509e62904a055f085579aed47fca1faa7a810f/Library/Formula/postgresql.rb

# move 9.4 away but keep it
$ brew unlink postgres

# install 9.5
$ brew install postgres
```

Now we can follow [Keita's instructions](https://kkob.us/2016/01/09/homebrew-and-postgresql-9-5/):

```sh
# initialize a new 9.5 db
$ initdb /usr/local/var/postgres9.5 -E utf8

# upgrade our data
$ pg_upgrade -d /usr/local/var/postgres -D /usr/local/var/postgres9.5 -b /usr/local/Cellar/postgresql/9.4.5_2/bin/ -B /usr/local/Cellar/postgresql/9.5.0/bin/ -v

# move 9.4 data away and the new data into place
$ mv /usr/local/var/postgres /usr/local/var/postgres9.4
$ mv /usr/local/var/postgres9.5 /usr/local/var/postgres

# remove 9.4
$ brew cleanup
```

And for good measure:

```sh
$ gem uninstall pg
$ gem install pg
```

