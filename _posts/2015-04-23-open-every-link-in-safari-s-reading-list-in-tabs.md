---
title: Open every link in Safari's Reading List in tabs
layout: post
tags:
  - applescript
  - ruby
---
**Use Safari's Reading List as an _inbox_.**

When you come across some link or page that you would like to look into at some point just not right now, hit `cmd+shift+d` on your mac or use the share sheet available almost everywhere on your iOS device and add it to Reading List.

Then when the time is right explode that thing into separate tabs and purge through them.

```ruby
#!/usr/bin/env ruby
# $ gem install CFPropertyList
require 'cfpropertylist'

path = File.expand_path '~/Library/Safari/Bookmarks.plist'
plist = CFPropertyList::List.new file: path

list = plist.value.value["Children"].value.select do |item|
  if title = item.value["Title"]
    title.value == 'com.apple.ReadingList'
  end
end.first.value["Children"].value

bookmarks = list.map do |item|
  item.value["URLString"].value
end.reverse

puts "Opening #{bookmarks.count} tabs "

bookmarks.each do |url|
  `osascript -e 'tell application "Safari" to tell window 1 to make new tab with properties {URL:"#{url}"}'`
  print '.'
end

puts ''
```

_Clear all items_ in Reading List and just re-add any linkif you're still undecided about it.

The script also works perfectly inside _Run Shell Script_ in Automator if you want a .app.

