---
title: Convert transparent WebP video to HEVC MP4 with alpha channel
layout: post
tags:
  - webp
  - mp4
  - ffmpeg
---
Converting the other direction [is widely documented](https://rotato.app/blog/converting-videos) but going in the other direction required a little more digging. Here's how I did it:

```sh
$ mkdir frames
$ ffmpeg -vcodec libvpx-vp9 -i INPUT_FILE.webm -pix_fmt rgba frames/%04d.png
$ ffmpeg -r 24 -i frames/%04d.png -c:v prores_ks -pix_fmt yuva444p10le OUTPUT.mov
```

That'll give you a ProRes4444 version of it. To convert further, I used a macOS built-in service.

1. Right-click on `OUTPUT.mov`
2. Select `Services > Encode Selected Video Files`
3. Select `HEVC 1080p` as the setting and check `Preseve Transparency`

This should give you a `OUTPUT-1.mov` which you can rename to `.mp4`.

Using transparent video is [easy and widely supported these days](https://rotato.app/blog/transparent-videos-for-the-web). Fun!
