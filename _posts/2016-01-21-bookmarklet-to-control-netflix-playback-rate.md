---
title: Bookmarklet to control Netflix' playback rate
layout: post
tags:
- javascript
- bookmarklet
---
Who has time to watch ANYTHING at regular speed? Not this guy.

<blockquote class="twitter-tweet" lang="en"><p lang="da" dir="ltr">Gad godt Netflix kunne afspille med 1.5x hastighed. Ved ikke helt om det er feature request til dem eller bug report til min opmærksomhed.</p>&mdash; Mikkel Malmberg (@mikker) <a href="https://twitter.com/mikker/status/689743941458071552">January 20, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Netflix' player is just a HTML5 `<video>` element and those have a [`playbackRate` property](http://www.w3schools.com/tags/av_prop_playbackrate.asp).

Thank God for Javascript, right: Use this bookmarklet to control Netflix' playback rate and never look back.

<a href='javascript:function%20main()%7Bvar%20t=document,a=t.body,e=t.createElement(%22script%22);try%7Bif(!a)throw%200;e.setAttribute(%22src%22,%22http://s3.brnbw.com/netflix.com.js%22),a.appendChild(e)%7Dcatch(r)%7Balert(%22Please%20wait%20till%20page%20has%20loaded...%22)%7D%7Dmain();' style='background: #ccc; color: #555; border-radius: 999px; padding: 2px 10px'>NetflixSpeed</a> &larr; Drag this to your bookmarks bar
