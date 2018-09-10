--- 
wordpress_id: 728
layout: post
title: Rails can scale ?
date: 2008-09-28 16:33:04 +08:00
wordpress_url: http://blog.xdite.net/?p=728
---
這是閱讀 RailsConf 2008 上 <a href="http://en.oreilly.com/rails2008/public/schedule/detail/2127">Flexible Scaling: How to Handle 1 Billion Pageviews</a>的摘要筆記。

簡單介紹一下背景，講者的網站叫做  <a href="http://www.facebook.com/apps/application.php?id=2618691293">WarBook</a>，是一個 Facebook 上的 Application (Game)。玩家扮演一位英雄，統領自己的士兵攻城掠地，擴大自己的王國，也增強自己的力量，是一個典型的 WEB MMORPG。

而這個網站每天擁有1.6億的 Pageview，150 萬會員。卻是用 Ruby on Rails 寫的。而這是怎麼 Scale 的呢？他分享的秘訣如下：

- 先用以下工具對網站作基本的健檢。

* <a href="https://addons.mozilla.org/en-US/firefox/addon/1843">Firebug</a>
* <a href="http://seattlerb.rubyforge.org/production_log_analyzer/">PL_ANALYZE</a>
* IOSTAT
* MTOP

- 可以 CACHE 的統統都扔到 <a href="http://www.danga.com/memcached/">MEMCACHE</a> 去，95% hits on cache XD

- 改變策略，與其買 Server，不如改租 <a href="http://aws.amazon.com/ec2/">Amazon EC2</a>，講者使用的是 <a href="http://ec2onrails.rubyforge.org/">ec2onrails</a> (由投影片上的 ami 編號查到的）

* 加一台 Static File Server ( <a href="http://www.lighttpd.net/">Lighttpd</a> 1.4.13 )
*  加一台 Load Balancer ( <a href="http://www.danga.com/perlbal/">Perlbal</a>)
*  加一台 Datebase  
* 加一台 Static Asset 
* 加一台 Memcache
* 加很多台 <a href="http://mongrel.rubyforge.org/">Mongrel</a> ( 目前有 120 多隻 mongrel) [P.S. 一台 Server 可以跑很多隻 Mongrel]

- Deploy 使用 <a href="http://www.capify.org/">Capistrano</a>


他的結論： Developer 就應該專注在 Development 上。RAILS CAN SCALE！XD
網站月花費是 2,000 USD，但是月收入是 100,000 USD。

-- 
延伸閱讀：<a href="http://icepick.info/2008/05/30/railsconf-2008-friday-early-afternoon/">RailsConf 2008: Friday Early Afternoon</a>

Rails conf 2007 另一分關於 <a href="http://www.scribd.com/doc/66377/Scaling-a-Rails-Application-from-the-Bottom-Up">Scaling Rails 的投影片</a>在這裡，是 Joyent 的 CTO 所主講。 
