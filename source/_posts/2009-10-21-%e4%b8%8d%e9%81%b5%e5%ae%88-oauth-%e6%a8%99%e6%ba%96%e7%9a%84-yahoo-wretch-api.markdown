--- 
wordpress_id: 1479
layout: post
title: !binary |
  5LiN6YG15a6IIE9BdXRoIOaomea6lueahCBZYWhvbyAvIFdyZXRjaCBBUEkg

date: 2009-10-21 16:53:49 +08:00
wordpress_url: http://blog.xdite.net/?p=1479
---
這是前幾天開發<a href="http://imexpert.tw"> I'm Expert </a>時的心得感想。

<a href="http://imexpert.tw"> I'm Expert </a>原本的重頭戲，其實是打算透過 OAuth 的方式，將文章同步到無名小站去。但現在看到的同步到 Wordpress / PIXNET (皆使用 XMLRPC）這個功能。完全是因為時限之內作不出來，才想到的變通之道。

（我是比賽結束當晚才試出來的）

如果是個人實力因素，那完全認栽沒話說。但在經過好幾晚的徹夜研究加上到現場找無名 RD 直接討論之後，才發現作不出來根本不是我的問題。

（雖然很感謝現場幫助我的無名 RD，但我還是要幹角一下 -_-）

寫 Yahoo OAuth 以及 Wretch OAuth 幾個的困難點在於

<blockquote>1. Yahoo OAuth 不 follow OAuth standard.
2. Wretch 更不 follow Yahoo spec ( 要正確能動，php 開發者甚至要改掉 yahoo sdk 的 Yahoo.inc 檔裡面的 post function，沒有人知道這個 hack，官網上也沒有）
3. Wretch 某些 API 一直到比賽當天，根本都還是爛的。（10/1 公開發表，10/17 還是爛的）
4. 只要錯誤，一律噴 404，完全無法 debug。
5. 沒人看得懂 WretchAPI 的 document 要表達什麼...
</blockquote>


而要解決這些問題：

1. 如果你是使用 Rails，<a href="http://github.com/pelle/oauth-plugin">oauth-plugin</a> 已經拿掉了 <a href="http://github.com/pelle/oauth-plugin/commit/339e807d4106a0c81e51a1380fe3843fe7791b19">YahooToken</a>的設計。執意想要開發這個功能的開發者，必須自己 rollback 回來，然後基於  implement SocialAPI 的  形式自己寫出 WretchAPI。拿掉的理由， <a href="http://stakeventures.com/articles/2009/10/06/why-yauth-is-not-oauth-4">plugin 開發者寫在這裡</a>，Yahoo OAuth 搞得他相當怒。

值得注意的是，Wretch post 吃 XML，而 YahooToken.rb 是用 json....，所以需要自己重寫 xml（get/post）的部份。我是用 xml-simple 作。

2. Wretch 沒有像 Social 一樣給出 guid 的 api 讓程式可以問，所以拿 guid 請自己想辦法 :/ 弔詭的是，wretch 的 API 都需要給 guid 才能拿/送資料。

3. php 開發者必須自行修改 yosdk.zip 裡的 Yahoo.inc 檔，把 post function 改成這一段

<script src="http://gist.github.com/214966.js"></script>

這段程式轉成其他語言版本的意義是

以開<a href="http://tw.developer.yahoo.com/wretch/api.php#albumService/[guid]/albums">新相簿</a>為例：

其實是要對 http://wretch.yahooapis.com/v1/albumService/ZKWHF6P5LXDZSICROTM76OS4CI/albums?<strong>class_id=1</strong> 丟 以下內容的 post

<script src="http://gist.github.com/214970.js"></script> 

========                                                                               

大致上技術上的 issue 都在這裡...。不過如果你真的想 implement 成商業產品，最讓開發者困擾的並不是這些，而是 Y! OAuth token 極容易 expire，大約 30 min 之內就會 expire。只要 token expire 掉就必須請使用者 reconnect ....

這才真正是令人抓狂的地方。比 BBAuth 還要更煩人 -_-

使用前請三思..

