--- 
wordpress_id: 1054
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x8D\x81\xE4\xBA\x8C\xE7\xAB\xA0 Jesse Newland & Deployment"
date: 2009-03-20 09:00:03 +08:00
wordpress_url: http://blog.xdite.net/?p=1054
---
Jesse Newland 在 RailsMachine 工作，<a href="http://railsmachine.com/">RailsMachine</a> 是一家 Rails Hosting 公司，所以他也對 Scale Rails Application 相當有經驗。

Jesse Newland 也給了三個 Scaling Rails Application 的 Tips

<blockquote>1. Separate / Isolate Your Rails Stack
2. Avoid Hitting the Database
3. Use an Intelligent Reverse Proxy</blockquote>
 
第一點: Islolating your Stack ：要把 Service 拆開放在不同 Server 上，以免其中一個拖垮其他。
<a href="http://www.flickr.com/photos/xdite/3338262008/" title="Flickr 上 xdite 的 vlcsnap-417580-crop"><img src="http://farm4.static.flickr.com/3615/3338262008_d696110944_m.jpg" width="183" height="240" alt="vlcsnap-417580-crop" /></a><a href="http://www.flickr.com/photos/xdite/3338262614/" title="Flickr 上 xdite 的 vlcsnap-417679-crop"><img src="http://farm4.static.flickr.com/3598/3338262614_8f63773dd3_m.jpg" width="191" height="240" alt="vlcsnap-417679-crop" /></a>
第三點就是：上 HAProxy。

一般 Rails Application 都是這樣部署的，用 Apache 的 mod_proxy，dispatch 到後面數台 Mongrel 上。

<a href="http://www.flickr.com/photos/xdite/3337434831/" title="Flickr 上 xdite 的 vlcsnap-419367-crop"><img src="http://farm4.static.flickr.com/3647/3337434831_23a49346d4_m.jpg" width="240" height="237" alt="vlcsnap-419367-crop" /></a>

但這種作法有可能會遇到一種情形，有些 Mongrel 太操，有些卻太涼。 

<a href="http://www.flickr.com/photos/xdite/3337426859/" title="Flickr 上 xdite 的 vlcsnap-420389"><img src="http://farm4.static.flickr.com/3362/3337426859_ce30ba8f62_m.jpg" width="240" height="180" alt="vlcsnap-420389" /></a>

因此要上 HAProxy 降低這種情形的發生，總比純 Round Robin 好多了...。

<a href="http://www.flickr.com/photos/xdite/3337426945/" title="Flickr 上 xdite 的 vlcsnap-421312"><img src="http://farm4.static.flickr.com/3568/3337426945_cbf0725d8f_m.jpg" width="240" height="180" alt="vlcsnap-421312" /></a>

但有時候太賽了，上了 HAProxy 還是會發生有些 Server 太操，有些又太涼的情形：

<a href="http://www.flickr.com/photos/xdite/3337427033/" title="Flickr 上 xdite 的 vlcsnap-424146"><img src="http://farm4.static.flickr.com/3414/3337427033_402923e6f1_m.jpg" width="240" height="180" alt="vlcsnap-424146" /></a>

Jesse Newland 的建議解法是把 maxconn 設為 1 。排不進去的就先 Queue 住，等誰有空了再丟給它。

<a href="http://www.flickr.com/photos/xdite/3337427093/" title="Flickr 上 xdite 的 vlcsnap-424750"><img src="http://farm4.static.flickr.com/3578/3337427093_9f83b3967a_m.jpg" width="240" height="180" alt="vlcsnap-424750" /></a>

<a href="http://nginx.net/">Nginx</a> 也有一個 module 叫 <a href="http://brainspl.at/articles/2007/11/09/a-fair-proxy-balancer-for-nginx-and-mongrel">fair proxy</a>，可做到類似的事。

<a href="http://www.flickr.com/photos/xdite/3338257928/" title="Flickr 上 xdite 的 vlcsnap-425706"><img src="http://farm4.static.flickr.com/3559/3338257928_453e0a08d7_m.jpg" width="240" height="180" alt="vlcsnap-425706" /></a>

<a href="http://www.modrails.com/">Passenger</a> ( Apache mod_rails) with global queuing，也可以做到類似的事。

<a href="http://www.flickr.com/photos/xdite/3337427429/" title="Flickr 上 xdite 的 vlcsnap-426113"><img src="http://farm4.static.flickr.com/3541/3337427429_551421cc67_m.jpg" width="240" height="180" alt="vlcsnap-426113" /></a>

下一章是 Jim Gochee 的 Interver
