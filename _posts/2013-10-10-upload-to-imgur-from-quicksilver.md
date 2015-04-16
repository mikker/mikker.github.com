---
layout: post
title: "Upload to imgur from Quicksilver"
tags:
  - quicksilver
  - applescript
---

I've become very fond of attaching animated GIF's of new UI elements when I create pull requests at [Firmafon][]. Recording a part of the screen is very easy using Quicksilver's built-in screen recording and trimming features. Then when done, open the file in [GIF Brewery][] and make it a gif. I usually lock the FPS to 15 and use the Simple Palette.

So far so good, we have a gif. Let's put it somewhere we can reference. [Imgur][] is great and keeps the images around for a long enough time for our colleagues to see it on Github. Luckily a command-line client for imgur already exists so [grab imguru][] from Github and put it in `~/bin` (or anywhere you'd like).

Quicksilver knows Applescript and supports custom actions in `~/Library/Application Support/Quicksilver/Actions`, so open up AppleScript Editor.app and make a script with the following:

{% highlight applescript %}
using terms from application "Quicksilver"
  on open theFile
    try
      set filePath to POSIX path of theFile
      set theCommand to "~/bin/imguru '" & filePath & "'"
      return do shell script theCommand
    on error e number n
      return e
    end try
  end open
  
  -- Tell Quicksilver we only work with files
  on get direct types
    return {"NSFilenamesPboardType"}
  end get direct types
end using terms from
{% endhighlight %}

Save it as `Upload to imgur.scpt` or something like it and you might have to restart Quicksilver and you're there.

[Firmafon]: http://firmafon.dk
[GIF Brewery]: http://gifbrewery.com
[Imgur]: http://imgur.com
[grab imguru]: https://github.com/FigBug/imguru
