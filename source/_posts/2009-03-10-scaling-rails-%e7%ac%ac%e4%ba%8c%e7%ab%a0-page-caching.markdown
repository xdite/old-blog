--- 
wordpress_id: 1013
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xBA\x8C\xE7\xAB\xA0 Page Caching"
date: 2009-03-10 09:00:47 +08:00
wordpress_url: http://blog.xdite.net/?p=1013
---
Rails 有很多方式可以實做 Cache，最直覺的方式的使用 Page Caching。

Mongrel 可以提供 20-50 requests  / seconds ，換算成一天的量是 200 萬次的 hits，這種量足夠一般網站用了。

<a href="http://www.flickr.com/photos/xdite/3336418862/" title="Flickr 上 xdite 的 vlcsnap-16563354"><img src="http://farm4.static.flickr.com/3301/3336418862_472caaf04e.jpg" width="500" height="375" alt="vlcsnap-16563354" /></a>

但如果將 Page Caching 選項打開的話，會產生這樣的改變：吐的結果將會變成  HTML，cache 在 Apache Server。這樣的作法可以提供更高的 throughput : 因為 Apache 可以提供 1000 requests / second，換算成一天的量就是 86 M hits。

<a href="http://www.flickr.com/photos/xdite/3336419016/" title="Flickr 上 xdite 的 vlcsnap-16564800"><img src="http://farm4.static.flickr.com/3602/3336419016_fc28810014.jpg" width="500" height="375" alt="vlcsnap-16564800" /></a>

實做 Page Catching 的方式在 Rails 相當簡單，只要到 config 裡，將

[Ruby]
cofing.action_controller.perform_caching = false
[/Ruby] 
改成
[Ruby]
cofing.action_controller.perform_caching = true
[/Ruby] 

修改 controller 

<a href="http://www.flickr.com/photos/xdite/3336431520/" title="Flickr 上 xdite 的 vlcsnap-16566917-crop"><img src="http://farm4.static.flickr.com/3376/3336431520_c978a1a492.jpg" width="443" height="263" alt="vlcsnap-16566917-crop" /></a>

可從 LOG 見到：把結果 cache 成 HTML 了。cache 的結果會放在 public 目錄下。

<a href="http://www.flickr.com/photos/xdite/3336434870/" title="Flickr 上 xdite 的 vlcsnap-16567175-crop"><img src="http://farm4.static.flickr.com/3626/3336434870_c80df6350c.jpg" width="383" height="158" alt="vlcsnap-16567175-crop" /></a>

<a href="http://www.flickr.com/photos/xdite/3336437696/" title="Flickr 上 xdite 的 vlcsnap-16568518-crop"><img src="http://farm4.static.flickr.com/3634/3336437696_2cbdba0118.jpg" width="162" height="257" alt="vlcsnap-16568518-crop" /></a>

但是現在有一個問題：當 Update 了內容，但這時候因為內容已經被 Cache 成  HTML 了。使用者這時候瀏覽並不會見到最新的內容。於是還要再作一件事：當 update 過後，我們必須 expire cache。

<a href="http://www.flickr.com/photos/xdite/3336436176/" title="Flickr 上 xdite 的 vlcsnap-16570650-crop"><img src="http://farm4.static.flickr.com/3642/3336436176_648eb4e25d.jpg" width="500" height="219" alt="vlcsnap-16570650-crop" /></a>

Page Caching 不只可 Cache 成 HTML 檔案，其他 format 如：xml , JSON 也支援，甚至可以自己 customize 檔案類型：如 iphone 網頁 XD。

<a href="http://www.flickr.com/photos/xdite/3335585029/" title="Flickr 上 xdite 的 vlcsnap-16572682"><img src="http://farm4.static.flickr.com/3361/3335585029_4c95f8e5f5.jpg" width="500" height="375" alt="vlcsnap-16572682" /></a>

Page Caching 的 Default Option 是放在 /public 下，有些人並不喜歡這個位置。Rails 提供了選項可自行指定位置：
[Ruby] 
config.action_controller.page_cache_directory = RAILS_ROOT + "/public/cache"
[/Ruby]

現在大家知道了 How to do it，但 When to do it？

Page Caching 比較適用的地方可能是  Blog 首頁、新聞網站首頁、Conference 首頁，具有變動較不頻繁性質（一天才需更新一次 ）的頁面。 

這章主要講述的是 Page Caching，下一章的內容是：Cache Expiration。
