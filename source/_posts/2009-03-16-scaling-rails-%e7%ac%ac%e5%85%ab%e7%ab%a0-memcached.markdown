--- 
wordpress_id: 1029
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x85\xAB\xE7\xAB\xA0 Memcached"
date: 2009-03-16 09:00:54 +08:00
wordpress_url: http://blog.xdite.net/?p=1029
---
什麼時候要用上 Memcached？

以 Blog 為例好了，也許 Cache 最直覺的作法就是用 Page Caching。但當站越來越大，比如說你設計了一個功能叫做「Friends」，這一頁會把你所有好友的 Recent Post 拉出來 render 成一頁。

也許想想，Page Caching 還是可行啊，但是這樣就會造成一個很蠢的結果。以 LiveJournal 為例，如果還是使用 Page Caching 的策略，就會製造出 17.5 million 的 firneds pages，因為 LievJournal 上就有 17.5 million 的使用者，簡直是令人無語。

<a href="http://www.flickr.com/photos/xdite/3337568912/" title="Flickr 上 xdite 的 vlcsnap-110145-crop"><img src="http://farm2.static.flickr.com/1019/3337568912_0463882921.jpg" width="500" height="288" alt="vlcsnap-110145-crop" /></a>

不過這樣似乎還是 doable 的。但是，當再繼續檢視下去呢，每一篇 Post 的 Display 是會去檢查 Security Level ( Public / Private / Group ) 的。每個人看到的頁面會依據權限的不同，產生不同的結果。因此會有 17.5 milliaon versions of 17.5 million friends pages，瘋了！

看起來使用 Action Caching 沒望，也不能指望 Action Caching 或 Fragment Caching 了。那還剩什麼解法呢？

但此時我們還有一種作法可解，把結果的 Array Cache 起來，丟進 memcache！

<a href="http://www.flickr.com/photos/xdite/3336735611/" title="Flickr 上 xdite 的 vlcsnap-115859"><img src="http://farm4.static.flickr.com/3315/3336735611_554d937512.jpg" width="500" height="375" alt="vlcsnap-115859" /></a>

如果已經 cache 了，就直接把結果丟回去。如果還沒，就從 database 裡撈。

那麼什麼是 Memcached 呢？這裡是比較冗長的版本： 

<a href="http://www.flickr.com/photos/xdite/3337571372/" title="Flickr 上 xdite 的 vlcsnap-116527-crop"><img src="http://farm2.static.flickr.com/1256/3337571372_ccc1ee39b6.jpg" width="500" height="119" alt="vlcsnap-116527-crop" /></a>

簡短的版本： {} （Hash) in Memory！

<a href="http://www.flickr.com/photos/xdite/3337572606/" title="Flickr 上 xdite 的 vlcsnap-117529-crop"><img src="http://farm2.static.flickr.com/1315/3337572606_67f49238bc.jpg" width="500" height="122" alt="vlcsnap-117529-crop" /></a>

如何在 Rails 裡使用 memcache 呢？在 config 裡指定：

[Ruby] 
confug.cache_store = :mem_cache_store
[/Ruby]

有兩種用法：

1. Object Store ( Like LiveJournal)
2. Fragment Cache Store ( Action & Fragment Caching)

<a href="http://www.flickr.com/photos/xdite/3336744763/" title="Flickr 上 xdite 的 vlcsnap-118211-crop"><img src="http://farm4.static.flickr.com/3386/3336744763_3bca101ef2_m.jpg" width="240" height="128" alt="vlcsnap-118211-crop" /></a>

<a href="http://www.flickr.com/photos/xdite/3337576336/" title="Flickr 上 xdite 的 vlcsnap-119778-crop"><img src="http://farm2.static.flickr.com/1173/3337576336_9b07b8da29_m.jpg" width="234" height="240" alt="vlcsnap-119778-crop" /></a>

開 script/console 看會比較清楚怎麼操作讀寫：

<a href="http://www.flickr.com/photos/xdite/3337577496/" title="Flickr 上 xdite 的 vlcsnap-120505-crop"><img src="http://farm2.static.flickr.com/1235/3337577496_14c406e3e3.jpg" width="452" height="409" alt="vlcsnap-120505-crop" /></a>

除了一般字串外，也可丟 ActiveRecord Object

<a href="http://www.flickr.com/photos/xdite/3336748013/" title="Flickr 上 xdite 的 vlcsnap-121155-crop"><img src="http://farm2.static.flickr.com/1044/3336748013_33f46bf803.jpg" width="500" height="255" alt="vlcsnap-121155-crop" /></a>

可用 fetch 的方式

<a href="http://www.flickr.com/photos/xdite/3336748481/" title="Flickr 上 xdite 的 vlcsnap-121322-crop"><img src="http://farm2.static.flickr.com/1083/3336748481_c7bbbc0c61.jpg" width="500" height="218" alt="vlcsnap-121322-crop" /></a>

避免處理 Cache Expiration，可以用 :expires_in 的方式作。

<a href="http://www.flickr.com/photos/xdite/3336736135/" title="Flickr 上 xdite 的 vlcsnap-121740"><img src="http://farm2.static.flickr.com/1201/3336736135_b0a05e29fc.jpg" width="500" height="375" alt="vlcsnap-121740" /></a>

除了在 Model 內做外，也可以在 View 裡面這樣作。這樣每三十分鐘才需要重新 query 一次。

<a href="http://www.flickr.com/photos/xdite/3337566076/" title="Flickr 上 xdite 的 vlcsnap-124717"><img src="http://farm4.static.flickr.com/3539/3337566076_57ff8bf5da.jpg" width="500" height="375" alt="vlcsnap-124717" /></a>

第二種 避免 Cache Expiration 的方式是用 Intelligen keys 。 但在講述此章節之前，要先深入理解一下 memcache 運作的方式。

運行 memcached -m 2000 時，memcached 會叫起 2 G 的記憶體供使用，當用滿的時候，memcached 夠聰明的會 pop stack ..。使用 memcached ，We can push data we never plan to expired。

<a href="http://www.flickr.com/photos/xdite/3336736369/" title="Flickr 上 xdite 的 vlcsnap-126239"><img src="http://farm2.static.flickr.com/1035/3336736369_be10684798.jpg" width="500" height="375" alt="vlcsnap-126239" /></a>

看看這個使用 Fragment Caching 的 view。其實可用加上 updated_at 的 timestamp 配合 memcached 弄出更好的作法。就可以省掉煩惱 Cache Expiration ...

<a href="http://www.flickr.com/photos/xdite/3337566342/" title="Flickr 上 xdite 的 vlcsnap-129840"><img src="http://farm2.static.flickr.com/1127/3337566342_d9293643d0.jpg" width="500" height="375" alt="vlcsnap-129840" /></a>

但每個需要 Fragment Caching 的 view 都需要這樣手寫 key 自己惡搞嗎？不，其實 Rails 已經內建了更方便的用法...

<a href="http://www.flickr.com/photos/xdite/3336762489/" title="Flickr 上 xdite 的 vlcsnap-131651-crop"><img src="http://farm2.static.flickr.com/1257/3336762489_88b6e19332.jpg" width="500" height="188" alt="vlcsnap-131651-crop" /></a>

但如果遇到以下這種需要拉大量有關於 Post 與 User 的狀況呢？其實是可以丟 Array 的，會自動產生這種型態的 key。還可指定用途，如 for sidebar 使用。

<a href="http://www.flickr.com/photos/xdite/3336763889/" title="Flickr 上 xdite 的 vlcsnap-133430-crop"><img src="http://farm4.static.flickr.com/3612/3336763889_871784fea7.jpg" width="500" height="252" alt="vlcsnap-133430-crop" /></a>

接下來就用 Flickr 來舉例吧。在 Fickr 上每單張的相片右下角都有一塊 "Additional Information"，當作者修改照片敘述或權限過後，才會需要更新。這種資訊就蠻適合這樣做的：

<a href="http://www.flickr.com/photos/xdite/3337596654/" title="Flickr 上 xdite 的 vlcsnap-134817-crop"><img src="http://farm4.static.flickr.com/3403/3337596654_437e29b78e.jpg" width="356" height="368" alt="vlcsnap-134817-crop" /></a>

又如相片下的 comment，又要怎麼實做 Cache 呢？作法：配合 comment 數量作 key：

<a href="http://www.flickr.com/photos/xdite/3337566954/" title="Flickr 上 xdite 的 vlcsnap-136941"><img src="http://farm2.static.flickr.com/1413/3337566954_b872687285.jpg" width="500" height="375" alt="vlcsnap-136941" /></a>

我們以上討論了怎麼實做，那麼什麼時候是適合用此法的時機：

1. 當使用 Action Caching 或 Fragment Caching 時。
2. 想減少 db hits 時。

<a href="http://www.flickr.com/photos/xdite/3337598016/" title="Flickr 上 xdite 的 vlcsnap-138512-crop"><img src="http://farm2.static.flickr.com/1170/3337598016_c9d7d90d16.jpg" width="500" height="244" alt="vlcsnap-138512-crop" /></a>

下一章是 <a href="http://www.engineyard.com/">EngineYard</a> 的 Staff ：Taylor Weibley 的 Interview。
