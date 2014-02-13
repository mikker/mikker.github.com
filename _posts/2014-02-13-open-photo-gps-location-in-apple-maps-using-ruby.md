---
layout: post
title: "Open photo GPS location in Apple Maps using ruby"
tags:
 - ruby
 - osx
---
Dr. Drang posted a [lengthy article and script](http://www.leancrew.com/all-this/2014/02/photo-locations-with-apple-maps/) to open a given photo's EXIF GPS location in Apple Maps. He's using python and a library called "PIL". I liked the idea but couldn't, no matter what I tried, get the damn thing to install and what is python anyway? I like ruby! I'm sure there's an easier way?

Turns out there was.

The rubygem `exifr` reads EXIF data like a champ, so let's get it:

````sh
$ sudo /usr/bin/gem install exifr
````

I'm using the absolute path to `gem`, because we want to end up using this as a system service, and system services use system ruby.

Now, here's the script. Save it as something like `~/bin/map.rb`:

````ruby
#!/usr/bin/env ruby
begin
  require 'exifr'
rescue LoadError
  require 'rubygems'
  require 'exifr'
end

usage = <<-USAGE
usage: map.rb IMAGE_PATH
USAGE

path = ARGV.shift

if path.nil?
  puts usage
  exit(0)
end

exif = EXIFR::JPEG.new(path)
if coords = exif.gps
  system "open 'http://maps.apple.com/?q=#{coords.latitude},#{coords.longitude}'"
else
  puts "No GPS data for #{path}"
end
````

Remember to `chmod +x` it and call it like the Doctor does in a service like this, substituting the path to where you saved the script:

![](http://farm4.staticflickr.com/3712/12409785794_6f3b45c9fd_o.png)
