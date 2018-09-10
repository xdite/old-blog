--- 
wordpress_id: 1789
layout: post
title: !binary |
  5L2/55SoIEFtYXpvbiBTMyArIEJpdFRvcnJlbnQg5rC45LmF5YiG5Lqr5qqU
  5qGI

date: 2010-07-10 23:21:54 +08:00
wordpress_url: http://blog.xdite.net/?p=1789
---
剛剛看到 朱宅學恆在<a href="http://www.plurk.com/p/69x2ew">他的噗浪</a>上面哀號他的主機每天的流量被開放式課程翻譯的演講用爆而束手無策。

<a href="http://www.flickr.com/photos/xdite/4780222054/" title="螢幕快照 2010-07-10 下午11.06.03 by xdite, on Flickr"><img src="http://farm5.static.flickr.com/4100/4780222054_e33916705e_o.png" width="444" height="91" alt="螢幕快照 2010-07-10 下午11.06.03" /></a>

我想一些人應該在巨大檔案的分享上應該也有類似的問題。其實如果<strong>手上有一點預算</strong>的話，這個問題是很好解決的。解法就是使用 Amazon 的 S3 服務搭配上 BitTorrent。

<a href="http://aws.amazon.com/s3/">Amazon S3</a> (Simple Storage Service) 是由 Amazon 所提供的線上儲存服務。以簡單的方式比喻就是他是一個穩定快速的永久線上儲存空間。把檔案扔上 S3，再把檔案永久網址貼出來，問題就解決了。

它的基礎收費是儲存空間，每 GB 0.15 美元。傳輸費用是 每 GB 0.1 美元。超過 50TB 以後的計費方式可以看價目表。

<a href="http://www.flickr.com/photos/xdite/4780239862/" title="螢幕快照 2010-07-10 下午11.13.55 by xdite, on Flickr"><img src="http://farm5.static.flickr.com/4094/4780239862_d8be9f144f.jpg" width="500" height="382" alt="螢幕快照 2010-07-10 下午11.13.55" /></a>

至於怎樣把檔案上傳到 S3，可以找一下支援 S3 的 FTP client。如果是 unix 主機的話，有 s3cmd 這種東西...

<big><strong>搭配 BitTorrent</strong></big>

當然有人還是會靠北，放 S3 讓大家自由下載很像不關水龍頭一樣，被人濫用了就很傷荷包。Amazon 的 S3 有設計一項機制，它會將你的檔案製作成種子。

比如說你的原始檔案位址是：
http://s3.amazonaws.com/marcel/kiss.jpg
那麼種子位址就會是：
http://s3.amazonaws.com/marcel/kiss.jpg?torrent

你可以選擇將 torrent <strong>另存新檔後再行發佈</strong>(可別笨笨的就將 ?torrent 的網址貼出去）。

這樣 Amazon S3 就可以充當檔案的永久種源，永遠不斷種。但是頻寬壓力卻不會都在你的 S3 帳單上。
