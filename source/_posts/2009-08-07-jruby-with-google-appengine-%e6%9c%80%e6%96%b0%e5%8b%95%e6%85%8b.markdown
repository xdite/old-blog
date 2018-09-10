--- 
wordpress_id: 1366
layout: post
title: "JRuby with Google AppEngine \xE6\x9C\x80\xE6\x96\xB0\xE5\x8B\x95\xE6\x85\x8B"
date: 2009-08-07 13:39:23 +08:00
wordpress_url: http://blog.xdite.net/?p=1366
---
大概在四月那時候，<a href="http://blog.xdite.net/?p=1138">Google AppEngine 推出了對 JAVA 的 support</a>。那時候我也花時間把玩了一下，最後也<a href="http://github.com/xdite/twitter-message-wall-gae/tree/master">用 JRuby + Sinatra + AppEngine 寫了一個 Demo Application</a>。

不過當時的心得，其實是有點被繁瑣的 deploy 設定、ORM、HTTP Library 打敗。所以之後有點懶得再去碰（小聲）。


不過趁今天放颱風假，把半個禮拜沒看的的 RSS 掃了一下。才發現最近在這一塊有了飛躍性的進展。

<a href=" http://rails-primer.appspot.com/"> http://rails-primer.appspot.com/</a>
<a href=" http://code.google.com/p/appengine-jruby/wiki/GettingStarted">http://code.google.com/p/appengine-jruby/wiki/GettingStarted</a>
<a href="http://www.infoq.com/cn/news/2009/04/datamapper-datastore-reggae">http://www.infoq.com/cn/news/2009/04/datamapper-datastore-reggae</a>

簡單的來說，就是把繁瑣的設定包得更簡單直覺，另外有人寫了 <a href="http://code.google.com/p/appengine-jruby/">Google App Engine API Wrappers for JRuby</a> 以及 <a href="http://github.com/genki/dm-datastore-adapter/tree/master"> DataMapper 對 DataStore 的 adapter </a>。 

Awesome!!!!!

=====
P.S. 當時 deploy 我要自己寫 script 搞 deploy、搞定一堆有的沒有的基礎環境、當時的 database wrapper 也不甚好用 ...orz 
