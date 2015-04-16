---
layout: post
title: "Getting your locales in line before going on your dokku adventure"
tags:
- dokku
- ubuntu
---

I'm really digging [dokku-alt](https://github.com/dokku-alt/dokku-alt "dokku-alt/dokku-alt"). Dokku is a simple way of setting up a deployment setup as easy as Heroku's. Dokku-alt is that plus some bundled plugins.

I had some troubles though with the Postgresql databases being created with ASCII encodings. So before you install and setup dokku get your locales in order - make your `/etc/locales` look like:

````sh
LANG=en_US.utf-8
LANGUAGE=en_US.utf-8
LC_CTYPE="en_US.utf-8"
LC_NUMERIC="en_US.utf-8"
LC_TIME="en_US.utf-8"
LC_COLLATE="en_US.utf-8"
LC_MONETARY="en_US.utf-8"
LC_MESSAGES="en_US.utf-8"
LC_PAPER="en_US.utf-8"
LC_NAME="en_US.utf-8"
LC_ADDRESS="en_US.utf-8"
LC_TELEPHONE="en_US.utf-8"
LC_MEASUREMENT="en_US.utf-8"
LC_IDENTIFICATION="en_US.utf-8"
LC_ALL=en_US.utf-8
````

Source it (or reboot) and then set up dokku.
