--- 
wordpress_id: 1113
layout: post
title: !binary |
  5YaN5qyh5oqK546pIEhlcm9rde+8iFJhaWxzIEhvc3RpbmcpIOeahOS4gOS6
  m+W/g+W+lw==

date: 2009-03-31 00:14:03 +08:00
wordpress_url: http://blog.xdite.net/?p=1113
---
<a href="http://heroku.com">Heroku</a> 是一個 Rails Hosting Sevice。剛推出的時候，以可以「在線上撰寫 Rails Application」（Ajax Console）這項神奇的 Feature 為著稱。

不過現在筆者撰寫 Web Application 的習慣一向是在 local 端寫好之後，配合 SCM （svn / git ）加上 Deploy Tool ( Capistrano）推出去。因此線上撰寫 Application 對我來說只能說是「炫而不實」XD 當初只把玩一下就放著了

今天聽老闆 <a href="http://hlb.yichi.org/blog/">hlb</a> 說，在 Heroku 上 Deploy Application 似乎變簡單了，便開張票要我練一下把某 Service 搬上去。

我大概玩了一下，整理了一些重點（詳細 Detail 可見他們的 <a href="http://heroku.com/docs">Document</a>）：

<blockquote>1. <strong>需要 *熟悉* Git 指令</strong>：

Heroku 目前的策略，取消了「在線上撰寫 Rails Application」。而改成類似 SCM + Deploy Tool 的方式。Heroku 有自己的 rubygems。而 git push heroku master 等於 git push + deploy。

2. <strong>必須手動撰寫 .gems 檔</strong> ：

<strong>require 的 gems 必須寫成一個 .gems 檔，一起 commit 上去</strong>，這樣 git push heroku 上去就會自動安裝。上面的 Rails 似乎是 2.2.2 版，所以如果 Rails 版本比較低（如 2.1.x），必須也要當成一個 require 的 rubygems 寫在 .gems 裡。我的建議是，如果想省時間，也許<strong>可以考慮把 rubygems unpack 成 plugin 的方式掛上去</strong>。

3. <strong>只支援 PostgreSQL</strong>：

Heroku 的內建 db 是 PostgreSQL，似乎沒有其他選擇。如果 Application 都是用 ActiveRecord 生 query 倒還好，如果有手刻 SQL 語法的份就必須要注意一下。有可以把舊有 db 推上去的選項，但我試用的心得是 MySQL 倒進去轉成 PostgreSQL 會遇上 schema_migration 版本號的 integer out of range 問題，直接導致 import 失敗。</blockquote>

結論：

Heroku 適合當 stagging / demo site ，而不適合當 production 環境。原因是 deploy 方式很便利，但效能似乎不佳以及有先天環境上的限制。而 Heroku 有<strong> DNS mapping，拿來當個測試用的站台不錯（自己找機器架好 Rails deploy 是一件累人的事）</strong>。以後如果在 <a href="http://www.railsenvy.com/">RailsEnvy</a> 上面看到什麼 powerful 的 opensource cms or application，可以扔上 Heroku 玩看看。
