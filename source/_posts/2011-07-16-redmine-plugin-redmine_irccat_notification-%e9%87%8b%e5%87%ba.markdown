--- 
wordpress_id: 2688
layout: post
title: "Redmine plugin \"redmine_irccat_notification\" \xE9\x87\x8B\xE5\x87\xBA"
date: 2011-07-16 18:14:38 +08:00
wordpress_url: http://blog.xdite.net/?p=2688
---
這是<a href="http://www.techbang.com.tw">我們團隊</a>裡面，用來把 redmine 的訊息丟上 irc 的 <a href="https://github.com/techbang/redmine_irccat_notifications">redmine plugin</a>。
原型是我在一年前寫的。同事 @<a href="http://twitter.com/v1nc3ntlaw">v1nc3ntlaw</a> 這兩個禮拜又將這隻 plugin 又做了更大程度的改寫。

現在 opensource 出來。

這是實際會打出來的 Log。
<script src="https://gist.github.com/1086227.js?file=gistfile1.txt"></script>

Feature:

- 編輯 wiki 時發送通知
- 新增 / 修改任何 ticket 時發送通知
- 可見狀態變化，以及委派者變化
- 可抽換 ircbot。（請改 speak method 一節，主要是使用 echo 然後 pipe 給 ircbot）
=====
原理：
hook redmine 內部的 callback，再 print 已有的 class 的資料。

好處：
團隊進行專案時，大家都會掛同一個 irc，很快就能知道剛剛踢出去的票，或是大家正在進行哪張票，主管或同事馬上的回覆。
