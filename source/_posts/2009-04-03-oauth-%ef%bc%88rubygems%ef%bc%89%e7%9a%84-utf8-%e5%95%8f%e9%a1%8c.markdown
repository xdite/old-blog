--- 
wordpress_id: 1121
layout: post
title: !binary |
  T2F1dGgg77yIcnVieWdlbXPvvInnmoQgdXRmOCDllY/poYw=

date: 2009-04-03 01:53:12 +08:00
wordpress_url: http://blog.xdite.net/?p=1121
---
最近在試玩 Twitter Oauth（不需要帳號密碼即能授權第三方站台存取 Twitter 內容）。也將以前的小作品 <a href="http://twitio.us">Twitio.us</a>（推書籤到 twitter）implement 上了，<strong>現在不需要打帳號密碼就可以收書籤</strong>。

<a href="http://www.flickr.com/photos/xdite/3405390980/" title="Flickr 上 xdite 的 Twitter"><img src="http://farm4.static.flickr.com/3449/3405390980_7cd21bc35e.jpg" width="500" height="207" alt="Twitter" /></a>

前晚在本機嘗試無誤（順便 Upgrade 上 Rails 2.3 ），昨天下午跑完 db migrate 上線以後非常開心。但隨之而來就是嚴重的災情，許多使用者紛紛抱怨認證沒有問題，但收書籤會 500 Internal Server Error....

我百思不得其解，翻了 log 找了很多 case test，才發現中文訊息是元兇。本來想花一點時間 patch 掉 plugin 應該就 OK 了。結果花了很多時間測試之後，才發現問題點不是出在 <a href="http://github.com/mbleigh/twitter-auth/tree/master">TwitterAuth</a> 這個 plugin 。<strong>是 Oauth 這個 gem</strong> ...

當 strategy 採用 oauth 時，post 是去呼叫 gem Oauth 的 post 而不是 TwitterAuth 的 post（亦即怎麼改都沒有用）。然後 Oauth 0.3.2 這個版本有 utf8 問題，post 送出中文會爛。在 <a href="http://groups.google.com/group/oauth-ruby/browse_thread/thread/58b4b581dece9b2a">Google Group : Oauth Ruby 找到有討論串提到這個問題</a>，作者在 stable 版出了以後才修了這個 bug。因此解法<strong>就是上 github 把 <a href="http://github.com/pelle/oauth/tree/master">ouath （trunk 版）</a>抓回來倒到 vendor/plugins 下就可以解掉這個問題了.....</strong>

媽的，還我的青春啊.....

----

承諾的 TwitterAuth 教學過一兩天再發，今天為了解這個 bug 腦細胞已經快死光了。
