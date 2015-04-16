---
layout: post
title: "MobilePay's undocumented URL schemes"
tags:
 - ios
---
I made a [request on
Twitter](https://twitter.com/mikker/status/395153819904270336) for url schemes
in the Danish app for easy mobile money transfers
[MobilePay](http://www.danskebank.dk/da-dk/privat/selvbetjening/produkter/pages/mobilepay.aspx).
Turns out it already had
[some](https://twitter.com/f0gh/status/395155698101026816).

````
mobilepay://send?amount=100&phone=88888888
mobilepay://request?amount=100&phone=88888888
````

Perfect! This makes for perfect [Launch Center
Pro](http://contrast.co/launch-center-pro/) actions.

![Action](/images/2013-10-30 at 16.02.33.png)
