--- 
wordpress_id: 2241
layout: post
title: "Rails Performance Tuning (2) - \xE5\x9C\xA8 partial \xE8\xA3\xA1\xE9\x9D\xA2\xE4\xB8\x8B query \xE5\x86\x8D\xE5\x81\x9A cache"
date: 2011-05-06 14:10:37 +08:00
wordpress_url: http://blog.xdite.net/?p=2241
---
在一些寫 Rails 的指南裡面會教你，標準的作法是在 controller 裡把要用的資料 query 出來。 如果是全站都會用到的東西就在 application controller 裡拉。

OK。但是這些作者也許忽略掉一個情形，如果是 sidebar 或者是 header menu 要怎麼處理？

for example
(1)
<a href="http://www.flickr.com/photos/xdite/5692657318/" title="螢幕快照 2011-05-06 下午2.05.20 by xdite, on Flickr"><img src="http://farm4.static.flickr.com/3589/5692657318_14fdfbbfcd_o.png" width="839" height="108" alt="螢幕快照 2011-05-06 下午2.05.20"></a>
(2)
<a href="http://www.flickr.com/photos/xdite/5692087577/" title="螢幕快照 2011-05-06 下午2.05.31 by xdite, on Flickr"><img src="http://farm3.static.flickr.com/2079/5692087577_a3b6756d02.jpg" width="368" height="494" alt="螢幕快照 2011-05-06 下午2.05.31"></a>

如果你真的這樣做了，<strong>因為這些資料都是「算」出來的</strong>。如果放在 application controller 肯定全站慢到不行。

解法：



<blockquote><strong>break rule，在 partial view 裡面下 query。外面再打一層 cache</strong>。</blockquote>

