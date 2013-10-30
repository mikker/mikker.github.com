---
layout: post
title: "Get the true mime type of a file in Ruby"
tags:
 - ruby
---
I was kind of disappointed to find out that the ruby [mime-type gem](http://mime-types.rubyforge.org) just looks at the file extension instead of getting the true mime type. This can cause trouble. Say we're having the user upload files to our webpage - but we only want .mp3s. Without checking the true mime-type the user will be let through if he just renames his file to .mp3.

If you're lucky enough to be on Unix though, you can get the true mime type by using the shell command `file`.

```` ruby
true_mime_type = `file --mime -b mp4-renamed-to.mp3`.chomp
  # => video/mp4; charset=binary
````

There we are.
