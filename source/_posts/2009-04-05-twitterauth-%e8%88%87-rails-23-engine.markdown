--- 
wordpress_id: 1126
layout: post
title: "TwitterAuth \xE8\x88\x87 Rails 2.3 : Engine"
date: 2009-04-05 20:30:55 +08:00
wordpress_url: http://blog.xdite.net/?p=1126
---
前幾天，我為之前開發的 Twitter 推書籤服務 <a href="http://twitio.us">Twitio.us</a> 上了 Twitter Oauth。以往使用 Twitter 第三方應用服務，多數情形都須將<strong>帳號密碼</strong>存在該網站上給程式使用。但隨之而來的風險是，你不知道第三方服務的站長，是否會拿到你的帳號密碼之後惡搞...。

而這就是 Twitter 最近推出 Oauth 認證機制的原因。當第三方網站使用 Oauth 與 Twitter 接軌，使用者不需要再輸入 Twitter 的密碼，而<strong>只需授權第三方應用服務，就可達到一樣的作用</strong>。

<a href="http://twitio.us">Twitio.us</a> 也是 based on Ruby on Rals。因為認證系統從 plugin : <a href="http://github.com/technoweenie/restful-authentication/tree/master">restful_authenication</a> + gem: twitter ，變更為 plugin: <a href="http://github.com/mbleigh/twitter-auth/tree/master">twiitterauth</a> ( 實現 twitter oauth) 的緣故。所以我也花了一點時間從 Rails 2.2 upgrade 到 Rails 2.3（這個 plugin 需要在 Rails 2.3 的環境才能使用）。

安裝說明，README 都會有。大致上說一下  跑完 script/generate twitter_auth 需要注意的地方：



<blockquote>1. 生出來的 config/twitter_auth.yml，內填的 secret <a href="http://twitter.com/oauth_clients/new">要上 Twitter 官方申</a>請。

2. <strong>如果原先就有 User 這個 Model</strong>，需要看一下這個 plugin 生的 migration 裡面的 column name，自己寫一個新的 migration 轉 db schema。

3. 不需要自行 merge controller 與 route。

以往實做一些特殊驗證的時候，開發者往往是拿一個簡單的 authenication plugin，配合上 library 自己 implement & merge。但這個 plugin 最令人 impressive 的地方卻是，用了 Rails 2.3 : Engine 這個 feature 掛了就直接上。

什麼是 Engine 呢？以往在寫 Rails Application 時，想要撰寫/分享一些特殊的功能。往往只能透過 blog 教學或者架設一個 demo application，讓人 copy & paste 程式碼回去自行 merge 回去 controller 或 model 裡。<strong> Rails 2.3 Enigne 這項新 feature 可以讓你把別人寫的「整個網站」/「整個功能」掛起來當作一個 plugin (Rails applications that can be embedded within other applications)</strong>。

<a href="http://www.flickr.com/photos/xdite/3413751629/" title="Flickr 上 xdite 的 圖片 13"><img src="http://farm4.static.flickr.com/3581/3413751629_5e5e445749_o.png" width="242" height="365" alt="圖片 13" /></a>

這一張圖是 twitio.us 所使用的 plugin，可以看到 twitter-auth 有自己的 model / controller / view 還有 routes。一行 code 都不用 merge，就可以把這個功能直接掛上來用。</blockquote>



非常酷，建議有意開發 Twitter Application 的 Rails Developer 可以嘗試這個 plugin。
