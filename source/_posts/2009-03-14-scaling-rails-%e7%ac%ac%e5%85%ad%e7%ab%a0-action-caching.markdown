--- 
wordpress_id: 1022
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x85\xAD\xE7\xAB\xA0 Action Caching"
date: 2009-03-14 09:00:51 +08:00
wordpress_url: http://blog.xdite.net/?p=1022
---
在此章，你會學的是 Action Caching 的技巧。Rails 實做 Cache 的方式並不是只有 Page Caching 而已，因此在進入主題之前，要介紹 Rails 其他種的 Cache 方式。

Page Caching 儲存的地方是 Disk。而 Action & Fragment Caching，也就是此章和下章的主題，是使用 confiured cache。

Rails 在 Cache Store 方面提供了很多種選項：

<a href="http://www.flickr.com/photos/xdite/3336504991/" title="Flickr 上 xdite 的 vlcsnap-67463-crop"><img src="http://farm4.static.flickr.com/3611/3336504991_8cb84c3ef9.jpg" width="500" height="305" alt="vlcsnap-67463-crop" /></a>

Action Caching 需要配合 filter 去實做。以這張圖為例，如果沒登入就直接從 Apache 吐已經 cache 過的結果或將 client 重導到 Login 頁，通過驗證的再向 mongrel 要。

<a href="http://www.flickr.com/photos/xdite/3337343484/" title="Flickr 上 xdite 的 vlcsnap-83496"><img src="http://farm4.static.flickr.com/3595/3337343484_07633b7f7d.jpg" width="500" height="375" alt="vlcsnap-83496" /></a>

Rails default 的 cache_store 是 :momory_store，其實儲存的資料就是一個 Hash {} 。Good until you run of memory，缺點是不能在 processses 彼此之間互相 share。當你開了不只一支 mongrel 在跑的時候，這種策略其實特別不利，處理 Cache Expiration 的狀況讓人也很頭大....

<a href="http://www.flickr.com/photos/xdite/3336518403/" title="Flickr 上 xdite 的 vlcsnap-72971-crop"><img src="http://farm4.static.flickr.com/3622/3336518403_e0c456db2b_o.jpg" width="640" height="282" alt="vlcsnap-72971-crop" /></a>

所以他比較推薦的作法還是使用 :file_store 或 mem_cache_store。

mem_cache_store 將是之後章節的主題。

那麼哪一種類型的頁面適合 Action Caching 呢？"每個人都可在頁面上見到相同的內容，但都需要經過驗證"。例如： Google Groups。

<a href="http://www.flickr.com/photos/xdite/3337337156/" title="Flickr 上 xdite 的 vlcsnap-77217-crop"><img src="http://farm4.static.flickr.com/3591/3337337156_63c7735498.jpg" width="500" height="268" alt="vlcsnap-77217-crop" /></a>

還有一種是 Conditional Action Caching。

<a href="http://www.flickr.com/photos/xdite/3337339558/" title="Flickr 上 xdite 的 vlcsnap-78868-crop"><img src="http://farm4.static.flickr.com/3297/3337339558_3836243052.jpg" width="500" height="291" alt="vlcsnap-78868-crop" /></a>

現在你知道怎麼作 Action Caching，那麼什麼時候是使用時機呢？

<a href="http://www.flickr.com/photos/xdite/3337338142/" title="Flickr 上 xdite 的 vlcsnap-79663-crop"><img src="http://farm4.static.flickr.com/3086/3337338142_62b5299cd1.jpg" width="500" height="290" alt="vlcsnap-79663-crop" /></a>

就不需像上一章那樣用 Ajax 的骯髒作法，因為 Ajax load 也是需要實際去 call action 的。

下一章講的是 Fragment Caching 。
