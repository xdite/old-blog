--- 
wordpress_id: 590
layout: post
title: "[Rails] Restful Authentication \xE7\xB5\x90\xE5\x90\x88 OpenID 2.0 "
date: 2008-06-13 18:24:41 +08:00
wordpress_url: http://blog.xdite.net/?p=590
---
前陣子，<a href="http://handlino.com">敝社</a>推出了一個新網站 <a href="http://sudomake.com">Sudomake</a> (<a href="http://handlino.com/blog/2008/05/29/68/">介紹在此</a>）。

在實作這個網站會員登入機制時，不同於以往在 <a href="http://blog.ericsk.org/archives/868">Rails Project 上導入 OpenID 的做法</a> ( act_as_authenicated + 直接呼叫 ruby-openid )，我們直接用了比較新的 plugin - <a href="https://github.com/technoweenie/restful-authentication/tree">Restful Authentication</a>，並且結合 <a href="https://github.com/rails/open_id_authentication/tree">OpenID Authentication</a> plugin，改寫出自己的版本。

特點

* 支援 OpenID 2.0 
* 一個使用者可以使用多個 Openid 登入
* 可以輕易的結合 <a href="http://www.clickpass.com/home">ClickPass</a> 

因為這些基礎功能以及特點，都極可能成為在一個 希望使用 OpenID 的新 Project 的 feature。為了讓大家減少發明輪子的力氣，於是我就把這些部份抽出重新打包裝成一個懶人包了 XD。

<big> <big><strong>請到 Github 的 <a href="https://github.com/xdite/openid_pack/tree">openid_pack 專案首頁</a>自取。 
</strong></big></big>
解開就具有 Login System , OpenID (含上述特點）的功能了。

---
撰寫的參考文件；
* <a href="https://github.com/rails/open_id_authentication/tree">OpenID Authentication</a> 的 Readme 以及
* Dr.nic  <a href="http://drnicwilliams.com/2007/07/26/sample-app-rails-multiple-openids-per-user/">Sample Rails app: multi-OpenIDs per user</a> 

不過還是踩了不少地雷，另外為了配合 clickpass 與做出較漂亮的 REST 也改掉了許多 code ..
