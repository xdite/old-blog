--- 
wordpress_id: 1045
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x8D\x81\xE7\xAB\xA0 Client-side Caching"
date: 2009-03-18 09:00:00 +08:00
wordpress_url: http://blog.xdite.net/?p=1045
---
在前幾集，我們一直 focus 在 Server 上的 cache 。現在我們要來開始看 Cline Browser 上的 Cache。

有三個 headers 我們可以拿來搞鬼。

1. max-age
2. etag
3. last_modified。

先來看 max-age

加 max-age 的方式很簡單，只要在 action 裡面加一行
[Ruby]
 expires_in 10.minutes
[/Ruby]

這頁就會送出一個 header 

[Ruby]
 header["Cache-Control"] = "max-age=600"
[/Ruby]

就是 Content 在 600 秒內都是 valid 的，600 秒內都不用重抓。除非 broswer 送出 refresh 指令。

<a href="http://www.flickr.com/photos/xdite/3337719262/" title="Flickr 上 xdite 的 vlcsnap-214817"><img src="http://farm4.static.flickr.com/3574/3337719262_88072f19dc.jpg" width="500" height="375" alt="vlcsnap-214817" /></a>

再來看 etag。

etag 是什麼呢？其實 Rails 內建了自動生 etag 的功能，只是你可能不知道。基本上是這樣運作的：client 發送一個 request 給 Server，Server 收到 request 後，除了 render body 丟回去外，還會多丟一個 header["etag"] (一串 hash)回去。當 client 下次再來問 Server 時，除了丟 request 過來外，它還會順便丟 etag 過來，問這次 Server 新render 的 body 之 etag，是否跟上次的 etag 相同。相同的話，Server 只送 304 Not Modified 回去。叫 Client 開自己的 local cache 讀就好了。

<a href="http://www.flickr.com/photos/xdite/3336890153/" title="Flickr 上 xdite 的 vlcsnap-216365"><img src="http://farm4.static.flickr.com/3347/3336890153_de16906c14.jpg" width="500" height="375" alt="vlcsnap-216365" /></a>

好處就是 : Faster page loading for the client。但 Still the same speed for the server（因為 Server 還是要生body 出來，算一次 etag，再比有沒有 match )。

一般來說，在 Rails 生的 etag 都是 md5(body)，但在 Rails 2.2 推出了一個新 feature，可以 custom key ，可用在 models 上。舉例來說：如果這一頁只是要 show 單一 user 的單一資訊，etag 可改成吃 object。如果 etag 相同，直接回傳 not_modified，而不用 render 整個 body 再出來算 etag。好處就是 <strong>No rendering the view or additional queries</strong>。

<a href="http://www.flickr.com/photos/xdite/3336890577/" title="Flickr 上 xdite 的 vlcsnap-225465"><img src="http://farm4.static.flickr.com/3337/3336890577_a299dc528a.jpg" width="500" height="375" alt="vlcsnap-225465" /></a>

當然也可吃  array ...

<a href="http://www.flickr.com/photos/xdite/3336900195/" title="Flickr 上 xdite 的 vlcsnap-226121-crop"><img src="http://farm4.static.flickr.com/3400/3336900195_39c6149e55.jpg" width="500" height="233" alt="vlcsnap-226121-crop" /></a>

最後要看的是 last_modified。這還蠻像 etag 的互動模式，只是比的是 last_modified 的 date 。

<a href="http://www.flickr.com/photos/xdite/3337719966/" title="Flickr 上 xdite 的 vlcsnap-227658"><img src="http://farm4.static.flickr.com/3645/3337719966_a24c174ab9.jpg" width="500" height="375" alt="vlcsnap-227658" /></a>

Greg 建議其實可以將 etag + last_modifie 搭配起來一起做。

<a href="http://www.flickr.com/photos/xdite/3337730628/" title="Flickr 上 xdite 的 vlcsnap-229655-crop"><img src="http://farm4.static.flickr.com/3576/3337730628_599c6d1e39.jpg" width="500" height="208" alt="vlcsnap-229655-crop" /></a>

什麼是適合使用的時機？

<strong>當你正在使用  Fragment 或 Object caching ，而且你想提高 throughput，且想節省更多 CPU time</strong>。

好處是：<strong>會增進 Server Performance, 因為 query 數 與 render 數會降低</strong>。<strong>而且 Client 會開起來更快，因為吃的是 local cache </strong>。

 下一章的主題是：Advanced HTTP Caching。
