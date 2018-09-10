--- 
wordpress_id: 1094
layout: post
title: !binary |
  U2NhbGluZyBSYWlsc++8mue4vee1kA==

date: 2009-03-24 19:24:18 +08:00
wordpress_url: http://blog.xdite.net/?p=1094
---
連載完了，所以來寫一下 Summary：



<blockquote><strong>（第一章的重點）</strong>
一般來說花在 loading page 的時間會比較高，改善這個 CP 值比較高。可用 YSlow 和其提供的 tips 去 提升效率。

<strong>（第二到第八章的重點）</strong>
介紹各種 Cache 策略的作法：Page Caching、Action Caching、Fragment Caching。使用 Memcache 搭配 Fragment Caching 等等。

<strong>（第九章的重點）</strong>
許多網頁應用程式瓶頸都是卡 DB，因此善用 MySQL 的 EXPLAIN 去找出 Slow Query，並且挖掘是哪一段的程式碼（ORM語法）造成的。記得要打 INDEX，將 counter 做 cache。上 Master Slave 架構。

<strong>（第十到第十一章的重點）</strong>
講 Client-side Caching。要提高速度，就是避免去 Hit DB，因此做好 Client-site Caching 是很重要的。這兩章主要是介紹三種 HTTP header：max-age、etags、last_modified 降低 Client 來問的次數。並且建議將流量轉嫁到 ReverseProxy 上。

<strong>（第十二章）</strong>
負載平衡以及如何實作簡單的 HA 架構。</blockquote>



其實不光是 Rails，一般 Web Application 「基本」的 Scaling 也大概都是照這些方式。如果對 Scaling 有興趣的話，這些主題是相當好的 Google 關鍵字...繼續挖掘下去，相信您會收穫更多的。
