--- 
wordpress_id: 1250
layout: post
title: !binary |
  WWVsbG93UGFnZXMuY29tIC0tIOS9v+eUqCBSdWJ5IG9uIFJhaWxzIOmAsuih
  jCBCaWcgUmV3cml0ZSDog4zlvoznmoTmlYXkuos=

date: 2009-05-31 03:03:32 +08:00
wordpress_url: http://blog.xdite.net/?p=1250
---
最近放假，窩在看一些大型 Conference 的 Slides 和 Videos，翻到 Qcon 2008 的這場 Talk：<a href="http://www.infoq.com/presentations/straw-yellowpages">YELLOWPAGES.COM: Behind the Curtain</a>，覺得蠻有意思的，看完 talk 以候趁著印象新鮮把重點摘要下來。

一般人對於 Rails 既定的印象都是<strong>只能拿來 prototyping</strong>，或者是 <strong>startup 開站時搶快，將來網站長大了之後，再用其他語言進行改寫的選擇（</strong> 即使許多名列前茅的 Facebook Application 都是使用 Rails 撰寫，每天擁有上千萬的 Pageviews。開發者對於 Scale 這件事還是內心存疑。）。更不用說使用 Rails 來改寫大型 production site....。

然而，<a href="http://www.yellowpages.com">YellowPages.com</a> 卻這麼做了。

<blockquote>YellowPages.com 是 AT&T 旗下的一個事業（ local yellowpage )

＊ <a href="http://alexa.com">Alexa</a> 的排名在全球 600 名左右
＊ Daily Pageview 是每日 300 萬左右
＊ More than 48 million requests / day
＊ More than 1500 requests/ second
＊ 每日大概有 200 萬次的 Search
＊ 每個月有 2500 萬的 unique vistor
＊ 舊版純然使用 Java 開發 ( <a href="http://www.flickr.com/photos/xdite/3577994399/">架構圖</a> ）
＊ 新版 ( Ruby on Rais ) Since <strong>2007/07/04</strong></blockquote>

在新版本上線運行一年之後，YellowPages.com 的首席架構師 John Straw 道出了當初改版的背後故事以及技術細節。

<strong><big>為什麼要改寫？</big></strong>

<blockquote>＊ 原先的網站是由一群顧問在 2004-2005 年用 Java 寫的。寫完就跑了...
＊ Fundamental design problems 
＊ Code 大概有 125K 行，但是一堆 copy-and-modify 混雜  :/
＊ Absolutly No Test ( Code) !
＊ 想加新功能非常非常非常的困難 ....</blockquote>

於是就開始策劃改寫了 ...

<strong><big>改寫的目標</big></strong>

<blockquote>＊ 換掉 Java Application Server
＊ 重新設計介面
＊ 改寫的時候順便加功能</blockquote>

<strong><big>新站的架構需求</big></strong>

<blockquote>＊ Absolute control of urls - <strong>Maximize SEO crawl-ability</strong>
＊ No sessions: HTTP is stateless
＊ Be agile: write less code
＊ Develop easy-to-leverage core business services</blockquote>

<strong><big>當中經過的 Survey 過程 (  Ruby / Python )</big></strong>

2007/1 - 2007/3 花了三個月嘗試各種架構 ( Ruby / Python ) ，最後花了四個月用 Rails 撰寫了新版...

＊第一版 -> <a href="http://www.flickr.com/photos/xdite/3578056833/">Web Tier : Rails ( fat controller ) , Service Tier -> Python</a>
＊第二版 -> <a href="http://www.flickr.com/photos/xdite/3578873040/">Web Tier  : Rails ( thin controller) , Service Tier -> Python</a>
＊第三版 -> <a href="http://www.flickr.com/photos/xdite/3578074247/">Web Tier : Django , Service Tier -> Python</a>
＊最終版 -> <a href="http://www.flickr.com/photos/xdite/3578876006/">Web Tier  : Rails , Service Tier -> Rails</a>

<strong><big>最後選擇 Rails 的原因 </big></strong>

<blockquote>1. 他們 team 最好的 python developer 剛烙跑 Q_Q...
2. Platform maturity ( important )
3. Better automated testing integration ( also )
4. A clearer path to moving parts of it to C if necessary for performance ( also )
5. The development team simply felt more comfortable with it.</blockquote>

<strong><big> 機器配置的考量</big></strong>

不過還有一些事，因為是前所未有的改版。他們對以下這些問題也有疑問...

<blockquote>1. <strong>需要多少台機器</strong> 
  - 原先 Java 版 的架構是 21 台。Rails 版總共用了 23 台（ Web Tier 17 台、Service Tier : 6 台）。DB ( <a href="http://www.oracle.com">Oracle</a> ) 2 台。
  - 所有的機器都是裝 <a href="http://www.centos.org">CentOS</a> 5, 只有 DB 是裝 <a href="http://www.sun.com/software/solaris/">Solaris</a> ( John called Solaris “<strong>a mistake they wouldn’t repeat</strong>,” not because of any particular problems with it but because of a<strong> lack of system administrators in their organization who had experience with it</strong>. XDDDDDDDDDD). 
2. <strong>一台機器要跑幾隻 <a href="http://mongrel.rubyforge.org">mongrel</a> </strong>
  - Web tier: 16 隻 , Service tier: 30 隻
3. <strong>要準備多少記憶體給 <a href="http://www.danga.com/memcached/ ">memcached</a> 用</strong>
  - 4G
最後架構 ：
<a href="http://www.flickr.com/photos/xdite/3578447539/" title="Flickr 上 xdite 的 Yellow Pages - Site at launch"><img src="http://farm3.static.flickr.com/2484/3578447539_c6d446023d_m.jpg" width="240" height="176" alt="Yellow Pages - Site at launch" /></a>
 </blockquote>

<strong><big>Performance optimization</big></strong>

主要的 performance goal :

<blockquote>＊Sub-second home page load time
＊4-second average search time
＊Never dies

1. 考慮過 <a href="http://haproxy.1wt.eu/">HAProxy</a> and <a href="http://swiftiply.swiftcore.org/">Swiftiply</a> 但是最後選了 <a href="http://www.f5.com/">F5</a>。（因為他們手上有 F5 而且熟悉操作...)
2. 自己寫了 Mongrel Handler 把 request 挑出來從 Web Tier 直送到 Servie Tier 而不經過 Rails ...
3. 自己寫了 C library 去 parse search cluster 的結果轉成 hash
4. 在 Web Tier 方面，選了 <a href="http://www.kuwata-lab.com/erubis/">Erbuis</a> 去 render view
5. 遵照 <a href="http://http://developer.yahoo.com/performance/rules.html">Yahoo performance guidelines</a> 去 tune 前端 ( 比如說 minified javascript, 把 css / js 包成一支 , 將 image 移到 <a href="http://www.akamai.com">Akamai</a> 上等等... ) , 將 <a href="http://www.prototypejs.org/">Prototype</a> 換成 <a href="http://jquery.com">jQuery</a>.
6.<strong> Apache was slow serving the 42-byte single-pixel GIFs </strong>that they use as analytics tags，他們去挖掘了原因之後換成了 <a href="http://nginx.net">Nginx</a>。( John 還氣憤給出了結論 : Apache is unsuitable for any production enviorment, in general XDDDDDDD) </blockquote>

經過這樣的 Tuning 以後，已經比以前 Java 版快上不少了，他們對這樣的改版結果很滿意。至於上線後前六個月，他們反而卡 DB 問題比較居多...

<strong><big>對於 Slow request 的解決之道</big></strong>

<blockquote>1. Slow requests in the web tier caused mongrel queueing
所以自己幹了 <a href="http://qrp.rubyforge.org">qrp</a> ( query reverse proxy ). Establish a backup pool where requests get parked until a mongrel is available （跟在 Nginx 上設 maxconn 為 1 ，其餘的 queue 住的作法類似）
2. Experimented with different malloc implementations 
3. Started using a custom MRI build -- ypc_ruby ( MRI 是指 Matz 版 Ruby 也就是一般 Ruby , 他們自己的 ypc_ruby 版本是對原版 Ruby 打上自己需要的 patch )
4. Started using a slightly-customized Mongrel</blockquote>



<strong><big> Ruby not Rails</big></strong>

<blockquote>1. 之後會嘗試把 Service Tier porting 到 <a href="http://merbivore.com">Merb</a> 去
2. Supporting development of Waves ( 他們 hire 了 <a href="http://rubywaves.com">Waves</a> 的開發者 )</blockquote>

<strong><big>改版完畢感想</big></strong>

<blockquote>滿意啊！！</blockquote>

<strong><big>觀眾提問</big></strong>

演講結束之後觀眾提了蠻多問題，也相當精采.. XD （太多了挑一些我覺得有興趣的整理出來...）

<blockquote>Q: 為什麼要用 Oracle ?
A: 因為 AT&T 有買 License..想不到什麼理由不用 XD

Q: 你們會 memory leaking 嗎？你們的 mongrel 的 uptime 多久...
A: 不會！我們沒有這種情形！不過每次我們 deploy 新版本的 code, 會重開一次，大概每隔兩週到一個月會這麼做...

Q: Team 人數 和 開發期?
A: Core Team 大概是四個人（其中只有一個在學校寫過 Rails)。當然還有部分是拆給其他人寫...。至於開發期的問題，在 AT&T 其他部門也有 project 改寫計畫，最後花了 24 個月。我們也以為要這麼久，但用 Rails 改寫 YelloPages 最後花了四個月.

Q: 你們用 ActiveRecord 嗎?
A: Yes

Q: 那表示你們的 db 需要 migrate 囉
A: Yes。重要的 model 有 migrate ...

Q: 你們的 Search ranking system ，有想轉 Lucene 嗎?
A: 我們從來沒用過 <a href="http://lucene.apache.org/java/docs/">Lucene</a>，但有想過用 Lucene，我們現在在用的是 <a href="http://www.fastsearch.com/">FAST</a>。不過不換成 Lucene 是因為我們如果想擁有自己掌控的 search engine，必須先去 hire 一個會寫 Lucene 的人，然後拜託他幫忙加上 Customer Ranking System 上去。但我們自己的 search cluster 已經夠快了 ..可以達到 3600 qps ...。但最終目標當然是想要擁有自己能掌控的 search engine。

</blockquote>

----
<strong><big>相關閱讀</big></strong>

<blockquote><a href="http://www.scribd.com/doc/13314829/YELLOWPAGESCOM-Behind-the-Curtain">YELLOWPAGES.COM: Behind the Curtain</a>（這場 Talk 的投影片）
<a href="http://www.360doc.com/content/081121/22/11192_1974622.html">The Rebuilding and Scaling of YellowPages.com</a>( 這場 Talk 的英文版重點摘要）
<a href="http://bmorearty.wordpress.com/2009/03/22/my-favorite-quotes-from-the-yellowpagescom-ruby-on-rails-talk/">My Favorite Quotes from the Yellowpages.com Ruby on Rails Talk</a> ( 外國鄉民對這場 Talk 的感想）
<a href="http://en.oreilly.com/rails2008/public/schedule/detail/2082">Surviving the Big Rewrite: Moving YELLOWPAGES.COM to Rails</a> ( RailsConf 2008 同一講者，類似主題，但內容面向不同）
<a href="http://www.slideshare.net/randquistcp/att-interactive-the-many-facets-of-ruby-presentation">At&T Interactive: The Many Facets Of Ruby</a> ( 這一份投影片有 YellowPages 當初轉換過程，一些詳細的架構和數據，值得一讀）</blockquote>

