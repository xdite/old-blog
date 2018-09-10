--- 
wordpress_id: 1027
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xB8\x83\xE7\xAB\xA0 Fragment Caching"
date: 2009-03-15 09:00:52 +08:00
wordpress_url: http://blog.xdite.net/?p=1027
---
什麼時候該用 Fragment Caching 呢：當一個頁面有很多不同區域都需要 cache 的時候。

<a href="http://www.flickr.com/photos/xdite/3337405764/" title="Flickr 上 xdite 的 vlcsnap-89672"><img src="http://farm4.static.flickr.com/3539/3337405764_e3b19e9878.jpg" width="500" height="375" alt="vlcsnap-89672" /></a>

<a href="http://www.flickr.com/photos/xdite/3336574369/" title="Flickr 上 xdite 的 vlcsnap-90814"><img src="http://farm4.static.flickr.com/3602/3336574369_fa0024ea13.jpg" width="500" height="375" alt="vlcsnap-90814" /></a>

實做方式是使用 partial

<a href="http://www.flickr.com/photos/xdite/3337535556/" title="Flickr 上 xdite 的 vlcsnap-91471-crop"><img src="http://farm2.static.flickr.com/1393/3337535556_d79368cfd8.jpg" width="500" height="207" alt="vlcsnap-91471-crop" /></a>

<a href="http://www.flickr.com/photos/xdite/3337534864/" title="Flickr 上 xdite 的 vlcsnap-91718-crop"><img src="http://farm4.static.flickr.com/3578/3337534864_ca6bc90bc4.jpg" width="500" height="269" alt="vlcsnap-91718-crop" /></a>

<a href="http://www.flickr.com/photos/xdite/3336707949/" title="Flickr 上 xdite 的 vlcsnap-93004-crop"><img src="http://farm4.static.flickr.com/3652/3336707949_2c113fc3c0.jpg" width="500" height="263" alt="vlcsnap-93004-crop" /></a>

但問題又來了，一樣也要處理 Cache Expiration 的問題。也是要到 Sweeper 去作 ...

<a href="http://www.flickr.com/photos/xdite/3336717501/" title="Flickr 上 xdite 的 vlcsnap-94062-crop"><img src="http://farm2.static.flickr.com/1376/3336717501_052d7c9582.jpg" width="500" height="209" alt="vlcsnap-94062-crop" /></a>

Fragment Method 在  controller layer  有這幾種。

<a href="http://www.flickr.com/photos/xdite/3336718105/" title="Flickr 上 xdite 的 vlcsnap-94610-crop"><img src="http://farm4.static.flickr.com/3640/3336718105_e383cbcb83.jpg" width="500" height="266" alt="vlcsnap-94610-crop" /></a>

以 Recent Area Event Photos 為例，Fragment 的名字可以這樣設計：

<a href="http://www.flickr.com/photos/xdite/3336575339/" title="Flickr 上 xdite 的 vlcsnap-96134"><img src="http://farm4.static.flickr.com/3541/3336575339_f81be0a946.jpg" width="500" height="375" alt="vlcsnap-96134" /></a>

個人 Event，可以這樣設計：

<a href="http://www.flickr.com/photos/xdite/3336575397/" title="Flickr 上 xdite 的 vlcsnap-96617"><img src="http://farm4.static.flickr.com/3663/3336575397_19b2019787.jpg" width="500" height="375" alt="vlcsnap-96617" /></a>
那什麼時候是需要使用 Fragment Caching 的時機呢：當你不能使用 Page Caching 或 Action Caching 時。比如說，頁面上有一些資訊跟其他 user 抓到的資訊相同，有些卻不同。

<a href="http://www.flickr.com/photos/xdite/3336575495/" title="Flickr 上 xdite 的 vlcsnap-97463"><img src="http://farm4.static.flickr.com/3329/3336575495_056ce7caca.jpg" width="500" height="375" alt="vlcsnap-97463" /></a>
影片的結尾，Greg 提到了 Cache Expiration 其實是一個很令人頭痛的東西。


<a href="http://www.flickr.com/photos/xdite/3337552870/" title="Flickr 上 xdite 的 vlcsnap-100788-crop"><img src="http://farm4.static.flickr.com/3634/3337552870_9fdc093b90.jpg" width="500" height="180" alt="vlcsnap-100788-crop" /></a>


假設網站使用 Fragment Caching ，
今天有一筆資料被刪除了，但是卻造成全站因此卻有幾百個的 page 需要 expire 掉，這實在太瘋狂了。

<a href="http://www.flickr.com/photos/xdite/3337407414/" title="Flickr 上 xdite 的 vlcsnap-101047"><img src="http://farm4.static.flickr.com/3339/3337407414_dd0511979e.jpg" width="500" height="375" alt="vlcsnap-101047" /></a>

還好，MEMCACHED 的誕生拯救了世界。下一章的主題就是它。

