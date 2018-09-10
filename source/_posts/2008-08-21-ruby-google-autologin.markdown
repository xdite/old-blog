--- 
wordpress_id: 649
layout: post
title: "[Ruby] Google AutoLogin"
date: 2008-08-21 02:13:18 +08:00
wordpress_url: http://blog.xdite.net/?p=649
---
最近因為要<del datetime="2008-08-20T17:58:17+00:00">幹一些壞事</del>需要分析研究一些資料，因此要接觸 Google 的 Auth API以取得存在 Google 上的資料。但令我有一點困擾的是，除了一些比較 Production 的產品可以開放用 /accounts/ClientLogin 方式回傳的 Token 取之外，<strong>一些尚在 Lab 但是需要登入的產品用此法卻無效</strong>。

研究了很久都沒有下文。最近在閒聊之中，就跟友人 <a href="http://cornguo.twbbs.org">Cornguo</a> 抱怨了此事，後來他丟了一篇有關於 <a href="http://www.askapache.com/webmaster/login-google-analytics.html">Google Analytics autologin</a> 的文章過來，我才豁然開朗要怎麼解。

原來我被制約了，先去看 <a href="http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html">Google Authentication 的 Help </a>乖乖取 Token 去 Try，而不是先去 google:// 惡搞一番。才沒有想到要用比較傳統的方式，直接幹 Cookie 來 ooxx ...。

送 POST 的位置也根其他的方式不太一樣： /accounts/ServiceLoginAuthBox 。



廢話不多說，直接貼做完的版本 ...

<script src="http://gist.github.com/6419.js"></script>
