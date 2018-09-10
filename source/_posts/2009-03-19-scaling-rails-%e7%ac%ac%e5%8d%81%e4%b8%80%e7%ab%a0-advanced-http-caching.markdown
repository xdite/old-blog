--- 
wordpress_id: 1050
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x8D\x81\xE4\xB8\x80\xE7\xAB\xA0 Advanced HTTP Caching"
date: 2009-03-19 09:00:01 +08:00
wordpress_url: http://blog.xdite.net/?p=1050
---
這章要討論的是： Advanced HTTP Caching。先從 Rack::Cache / Varnis / Squid / Akamai （ Revese Proxy Caches ）談起。 

Proxy Cache 應該大家都比較明白是什麼，我這裡就不多解釋了。

<a href="http://www.flickr.com/photos/xdite/3336960811/" title="Flickr 上 xdite 的 vlcsnap-245003"><img src="http://farm4.static.flickr.com/3598/3336960811_9195163a34.jpg" width="500" height="375" alt="vlcsnap-245003" /></a>

那什麼是 Reverse Proxy Cache。

有天，當你老闆找到你，向你抱怨 Server loading 太重了，但公司卻沒有錢能買更多的 Server 回來撐。這時候就是用上 Reverse Proxy Cache 的時候了。

是這樣運作的，A Client 第一次向我們的 Reverse Proxy 要資料，Reverse Proxy 上面沒有，於是 Server 生給 Reverse Proxy 一份，並 Cache 成 HTML 在 Reverse Proxy 上。因此當下一個 B client 來要這分資料時，就直接由 Reverse Proxy 吐給 B。Server 晾在後面喘氣。

<a href="http://www.flickr.com/photos/xdite/3336960867/" title="Flickr 上 xdite 的 vlcsnap-246676"><img src="http://farm4.static.flickr.com/3579/3336960867_217b140c10.jpg" width="500" height="375" alt="vlcsnap-246676" /></a>

<a href="http://www.flickr.com/photos/xdite/3336965987/" title="Flickr 上 xdite 的 vlcsnap-248257-crop"><img src="http://farm4.static.flickr.com/3299/3336965987_5d68ccbe75.jpg" width="500" height="260" alt="vlcsnap-248257-crop" /></a>

但 Reverse Proxy 與 Server 間的 Expiration 要怎麼處理呢？

<a href="http://www.flickr.com/photos/xdite/3337796358/" title="Flickr 上 xdite 的 vlcsnap-248634-crop"><img src="http://farm4.static.flickr.com/3343/3337796358_8ddc5f84f5.jpg" width="500" height="309" alt="vlcsnap-248634-crop" /></a>

這時候就用到上集講到的 max-age / etag / last_modified 了。

有三種方式可作：

max-age

<a href="http://www.flickr.com/photos/xdite/3336961021/" title="Flickr 上 xdite 的 vlcsnap-250060"><img src="http://farm4.static.flickr.com/3404/3336961021_5a559c4540.jpg" width="500" height="375" alt="vlcsnap-250060" /></a>

etags

<a href="http://www.flickr.com/photos/xdite/3336961059/" title="Flickr 上 xdite 的 vlcsnap-250605"><img src="http://farm4.static.flickr.com/3317/3336961059_1eebf12c51.jpg" width="500" height="375" alt="vlcsnap-250605" /></a>

last_modified

<a href="http://www.flickr.com/photos/xdite/3336961159/" title="Flickr 上 xdite 的 vlcsnap-251460"><img src="http://farm4.static.flickr.com/3389/3336961159_3e1beb343b.jpg" width="500" height="375" alt="vlcsnap-251460" /></a>

使用 max-age、etag、last_modified + Reverse Proxy 能有效的降低 Server 負擔，增進 website server 端速度。但問題還是存在，用 etag / last_modified 這些方式還是<strong>每個 request 都要來問一次 Server</strong>。要怎麼解決呢？

答案就是混在一起作 ....。

<a href="http://www.flickr.com/photos/xdite/3337801488/" title="Flickr 上 xdite 的 vlcsnap-253847-crop"><img src="http://farm4.static.flickr.com/3600/3337801488_00a9d1185a_m.jpg" width="240" height="102" alt="vlcsnap-253847-crop" /></a>

<a href="http://www.flickr.com/photos/xdite/3337791098/" title="Flickr 上 xdite 的 vlcsnap-254386"><img src="http://farm4.static.flickr.com/3321/3337791098_5ca674d1bf.jpg" width="500" height="375" alt="vlcsnap-254386" /></a>

<a href="http://www.flickr.com/photos/xdite/3336961427/" title="Flickr 上 xdite 的 vlcsnap-254476"><img src="http://farm4.static.flickr.com/3590/3336961427_a093003a17.jpg" width="500" height="375" alt="vlcsnap-254476" /></a>

max-age + etags Benchmark 數據相當不錯。3 百萬的 page requests ，Rails server 只被 hit 2 次 ...。

<a href="http://www.flickr.com/photos/xdite/3336961597/" title="Flickr 上 xdite 的 vlcsnap-255011"><img src="http://farm4.static.flickr.com/3542/3336961597_99455393cd.jpg" width="500" height="375" alt="vlcsnap-255011" /></a>

<a href="http://www.flickr.com/photos/xdite/3336961669/" title="Flickr 上 xdite 的 vlcsnap-255295"><img src="http://farm4.static.flickr.com/3597/3336961669_66990b51af.jpg" width="500" height="375" alt="vlcsnap-255295" /></a>

<a href="http://www.flickr.com/photos/xdite/3336961765/" title="Flickr 上 xdite 的 vlcsnap-255480"><img src="http://farm4.static.flickr.com/3550/3336961765_c426da8fc8.jpg" width="500" height="375" alt="vlcsnap-255480" /></a>

以 Akamai 提供的全球 Reverse Proxy 服務來說，可以幾乎把量都導到 Akamai 上 ...。

<a href="http://www.flickr.com/photos/xdite/3337791462/" title="Flickr 上 xdite 的 vlcsnap-256635"><img src="http://farm4.static.flickr.com/3392/3337791462_eb4106cd79.jpg" width="500" height="375" alt="vlcsnap-256635" /></a>

如果你想練習 Reverse Proxy 的話，可以先用 Rake::Cache 練習看看 （需要 Rails 2.3 )。

<a href="http://www.flickr.com/photos/xdite/3336961843/" title="Flickr 上 xdite 的 vlcsnap-258102"><img src="http://farm4.static.flickr.com/3604/3336961843_6839d04e84.jpg" width="500" height="375" alt="vlcsnap-258102" /></a>

問題又來了，每個 Rake::Cache 都是一台 Reverse Proxy，那不同 Process 要怎麼解決溝通問題？答案是可以用 File Store 或 memcache 解掉。

<a href="http://www.flickr.com/photos/xdite/3336961925/" title="Flickr 上 xdite 的 vlcsnap-258614"><img src="http://farm4.static.flickr.com/3661/3336961925_a3d8a93d9b.jpg" width="500" height="375" alt="vlcsnap-258614" /></a>

但目前 Rake::Cache 還有一個問題就是了，default header Cache-Controll
 是 private ，造成無法 proxy cache。造成一些 method 失效。

<a href="http://www.flickr.com/photos/xdite/3336961979/" title="Flickr 上 xdite 的 vlcsnap-259737"><img src="http://farm4.static.flickr.com/3333/3336961979_eddbc9fea7.jpg" width="500" height="375" alt="vlcsnap-259737" /></a>

為什麼 default 是 private 而不是 public ? 因為這個 header 不只告訴 Reverse Proxy，也告訴了 Proxy。如果 default 是 public 的話，那就會相當精彩了。Proxy 在 cache 時，就會把一些個人資訊 cache 在 Server 上，如果這些資訊是銀行帳戶，就大鑊了...XD。

<a href="http://www.flickr.com/photos/xdite/3337791740/" title="Flickr 上 xdite 的 vlcsnap-262796"><img src="http://farm4.static.flickr.com/3539/3337791740_2caafe8d08.jpg" width="500" height="375" alt="vlcsnap-262796" /></a>

還是有解法的，但要手動加就是了：

<a href="http://www.flickr.com/photos/xdite/3337791792/" title="Flickr 上 xdite 的 vlcsnap-263109"><img src="http://farm4.static.flickr.com/3659/3337791792_e91e64d8c1.jpg" width="500" height="375" alt="vlcsnap-263109" /></a>

不過 2.3 還沒 release（也許 release 時候就會誕生不這麼骯髒的解法），所以 Rake::Cache 真的當作練習玩具就好了。 

那什麼時候是使用 Reverse Proxy 的時機呢？<strong>當你把該做的 caching 都做完了，也上了 max-age, etags, last_modified。而且你的網站大概已經有一百萬會員的時候</strong> .....XD

下一集是： Jesse Newland 的 Interview
