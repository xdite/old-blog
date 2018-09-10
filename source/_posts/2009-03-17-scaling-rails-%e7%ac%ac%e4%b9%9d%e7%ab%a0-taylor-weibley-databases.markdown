--- 
wordpress_id: 1040
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xB9\x9D\xE7\xAB\xA0 Taylor Weibley & Databases"
date: 2009-03-17 09:00:58 +08:00
wordpress_url: http://blog.xdite.net/?p=1040
---
EngineYard 是一間 Rails Hosting 公司（品質很好，但收費很高），因此他們的員工常有機會接觸到寫的很爛的 Rails Code，對於怎樣 Tune 和 Scale up 特別有經驗....。

Taylor Weibley 給了三個建議 for Scaling Rails Application

<blockquote>1. Beware of fetching external data. 
頁面上的資料儘量不要依賴需要 external data 所提供的資訊。（會拖慢速度外、有可能會碰上未預期的錯誤）

2. Optimized your Database。
有些人會忘記打 index ...。你可以用去找出 Slow query 去改善它 ...

3. Design for Scaling Upfront。</blockquote>

在影片中 Taylor 提到了要 Speed up 有兩種作法，一個是 Optimize your Database 或 Architecture the database in the way you can sacle。

Greg 接續 Talyor 的理論深入解說 how to keep your database fast：

<blockquote>1. Don't forget indexes
可能是 key 的都要記得打 index。

<a href="http://www.flickr.com/photos/xdite/3337649402/" title="Flickr 上 xdite 的 vlcsnap-194058-crop"><img src="http://farm4.static.flickr.com/3655/3337649402_f09944048e_m.jpg" width="240" height="126" alt="vlcsnap-194058-crop" /></a>

2. Use Include。
此法可以減少多餘的 query，有沒有用 include 其實差很多，不要忘記檢視 ORM 自動生的 query 。

3. Use counter caching
總數不要暴力算的，相反地，應該多開一個欄位把 counter cache 起來。ActiveRecord 有個 :counter_cache => true 選項可以開。增刪資源會自動計數。

4. 安裝 QueryTrace Plugin
去搞清楚你的系統產生的那些噁心的 query，是哪一片段的 code 產生的。

5. Drop to SQL
不要害怕手刻 SQL。某些 query 用 ORM 自動生非常消耗資源（因為很不聰明）。用手刻的反而效率高。

6. Denormalizing Database Colunms
Denormalize 並不是什麼可恥的是，站長大了就是要這樣作啊，效率高很多的 .....

7. Slaves and Masters
db 上 Master / Slave 架構，有個 plugin 叫 <a href="http://github.com/technoweenie/masochism/tree/master">masochism</a> 可以做到這點。Master 用來寫資料，Slave 用來讀 ...
 
8. Databse Sharding。

<a href="http://www.flickr.com/photos/xdite/3337647522/" title="Flickr 上 xdite 的 vlcsnap-201113"><img src="http://farm4.static.flickr.com/3549/3337647522_713de65854.jpg" width="500" height="375" alt="vlcsnap-201113" /></a>

可用 <a href="http://github.com/fiveruns/data_fabric/tree/master">Data Fabric </a>這個  Gem 做到。</blockquote>

<a href="http://www.flickr.com/photos/xdite/3337647580/" title="Flickr 上 xdite 的 vlcsnap-201979"><img src="http://farm4.static.flickr.com/3585/3337647580_688ba018a8.jpg" width="500" height="375" alt="vlcsnap-201979" /></a>

----
最後如果你想學更進階的 ActiveRecord ，Greg 推薦你觀看他的另一部作品 <a href="http://envycasts.com/products/advanced-activerecord">Advance ActiveRecord</a> 學習更多有關 AR 的更多進階知識。

下一集的內容是 Client-Side-Caching。
