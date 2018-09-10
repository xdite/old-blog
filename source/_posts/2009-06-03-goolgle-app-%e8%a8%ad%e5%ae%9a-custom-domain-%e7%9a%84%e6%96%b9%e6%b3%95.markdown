--- 
wordpress_id: 1270
layout: post
title: !binary |
  R29vbGdsZSBBcHAgRW5naW5lIOe2geWumiBDdXN0b20gRG9tYWluIOeahOaW
  ueazlQ==

date: 2009-06-03 00:34:15 +08:00
wordpress_url: http://blog.xdite.net/?p=1270
---
今天晚上跟 <a href="http://itszero.org/">itsZero</a> 討論幹壞事的細節。聊起之前我用 <a href=" http://github.com/xdite/twitter-message-wall-gae/tree/master">jruby + sinatra + GAE</a> 寫的 <a href="http://gae.mrie6.com">MrIE6</a> ，一連才發現示範站 404 了。

呃。可是自從上線以來，我都沒有動過 DNS 設定啊，怎麼會莫名其妙 404 ... 後來才發現是 Google 又改架構了。以前直接可以在 GAE 直接設 CNAME。

研究了一下，發現現在的流程改成這樣：

1. 必須要先去註冊 <a href="http://www.google.com/a/cpanel/domain/new">Google Apps</a>, claim 我是 MrIe6.com 的 owner。
2. 然後要到 Domain Settings 去打開這兩項

Enable prelease features
<a href="http://www.flickr.com/photos/xdite/3588830795/" title="Flickr 上 xdite 的 圖片 19"><img src="http://farm4.static.flickr.com/3625/3588830795_b32d49af22_o.png" width="415" height="78" alt="圖片 19" /></a>

Next Generation
<a href="http://www.flickr.com/photos/xdite/3588830799/" title="Flickr 上 xdite 的 圖片 20"><img src="http://farm3.static.flickr.com/2450/3588830799_260458c0b6_o.png" width="452" height="73" alt="圖片 20" /></a>

3. 回到 Server Dashboard 可以 Add more services
<a href="http://www.flickr.com/photos/xdite/3589661640/" title="Flickr 上 xdite 的 圖片 21"><img src="http://farm4.static.flickr.com/3399/3589661640_c6434715f8_o.png" width="275" height="49" alt="圖片 21" /></a>

加入你的 Google Apps ( 輸入 app id )

<a href="http://www.flickr.com/photos/xdite/3588902071/" title="Flickr 上 xdite 的 圖片 25"><img src="http://farm4.static.flickr.com/3601/3588902071_827e9820a0_o.png" ></a>

4. 然後設定 URL 

<a href="http://www.flickr.com/photos/xdite/3588895503/" title="Flickr 上 xdite 的 圖片 24"><img src="http://farm4.static.flickr.com/3586/3588895503_40eceab5c2_o.png" width="631" height="119" alt="圖片 24" /></a>

再到域名商設定 CNAME 指到 ghs.google.com ....

<strong>真不是普通的囉嗦啊啊啊啊啊....</strong>

先記下來，以後這篇應該自己還會用到。
