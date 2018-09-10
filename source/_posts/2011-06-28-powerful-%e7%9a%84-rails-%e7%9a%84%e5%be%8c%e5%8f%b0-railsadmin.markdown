--- 
wordpress_id: 2559
layout: post
title: !binary |
  UG93ZXJmdWwg55qEIFJhaWxzIOmAn+aIkOW+jOWPsCA6IFJhaWxzQWRtaW4=

date: 2011-06-28 02:26:23 +08:00
wordpress_url: http://blog.xdite.net/?p=2559
---
前幾個禮拜幫<a href="http://rails-101.logdown.com">自己的書</a>刻官網，那時候已經有點精神不濟了，實在相當懶得自己手刻 CRUD 後台 以及實作 Authentication。當下就決定使用硬幹法...（雖然我手刻一個 CRUD 不需要 5 分鐘，但是那天真的累了）。多硬幹？基本上在網站上面看到的 view 和 route 都是我徒手硬寫的，沒有任何 model  ....

<strong>證明了人只要拋棄羞恥心，不只 PHP 可以寫得非常快，Rails 更可以寫的霹靂快？</strong>

後來一直想找個時間補個後台，但是想到 CRUD 就懶，畢竟我現在在公司已經連 CRUD 都不寫了，只負責研究底層和 cutting edge 的東西，和實作一些架構的 prototype。後來看到 RSS Feed <a href="http://www.viget.com/extend/rails-admin-interface-generators/">有一篇文章在介紹快速後台</a>。

文章主要提到了兩套快速後台 <a href="https://github.com/sferik/rails_admin">RailsAdmin</a> 和 <a href="http://activeadmin.info/">ActiveAdmin</a>。鑑於 ActiveAdmin 是新出的，而且精美異常...於是我就先嘗試了這一套。

<a href="http://www.flickr.com/photos/xdite/5877413615/" title="螢幕快照 2011-06-28 上午1.51.30 by xdite, on Flickr"><img src="http://farm6.static.flickr.com/5268/5877413615_80333b376b.jpg" width="500" height="318" alt="螢幕快照 2011-06-28 上午1.51.30"></a>


Demo 網站可以<a href="http://demo.activeadmin.info/admin">看這裡</a>。

翻了一下文件大致上是寫 DSL 去把整個後台做出來，寫起來非常的舒服。

但是在設計一些 model 欄位時，覺得自己有 checkbox 之類的需求，ActiveAdmin 不太夠用。於是就回去翻了一下看起來比較醜的 RailsAdmin 有什麼 feature。不翻還好，翻了覺得自己完全不應該以貌取人，直接 easygit revert 剛剛的 commit，重弄整個後台。

<a href="http://www.flickr.com/photos/xdite/5878025494/" title="螢幕快照 2011-06-28 上午2.06.42 by xdite, on Flickr"><img src="http://farm6.static.flickr.com/5262/5878025494_04019b34d7.jpg" width="500" height="283" alt="螢幕快照 2011-06-28 上午2.06.42"></a>

完全不知道什麼時候 <strong>RailsAdmin 變得這麼強大</strong>了，DjangoAdmin 根本是被一炮擊沉。

Demo 網站可以<a href="http://demo.railsadmin.org/">看這裡</a>。

簡單的感想就是：從前在 Rails 2 時代，的每一套快速後台 plugin，都只有相當差強人意的水準。多數只是去撈 database schema，然後把所有欄位都拉出來修改。完全只是個 phpMyAdmin 等級的後台，醜到爆炸不說，使用者 authenticated system 也各自為政。用這些 plugin customized 出自己的後台，想都別想。

而現今的這一套  <a href="https://github.com/sferik/rails_admin">RailsAdmin</a> 是 2010 Ruby Summer Code 的作品，想法和架構是從 MerbAdmin porting 過來的。由於 RailsAdmin 的時候已經是具有乾淨 API 的 Rails 3 時期，加上這一套的作者群裡面甚至有 Rails 架構師 Yehuda Katz 。架構實在漂亮的不得了。

這是我的後台操作介面。

<a href="http://www.flickr.com/photos/xdite/5877511287/" title="螢幕快照 2011-06-28 上午2.20.14 by xdite, on Flickr"><img src="http://farm6.static.flickr.com/5267/5877511287_9398672c0c.jpg" width="500" height="348" alt="螢幕快照 2011-06-28 上午2.20.14"></a>

而這是我為這個 model 寫的 DSL。

<a href="http://www.flickr.com/photos/xdite/5877511381/" title="螢幕快照 2011-06-28 上午2.20.33 by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6060/5877511381_3d18b77bb9.jpg" width="500" height="449" alt="螢幕快照 2011-06-28 上午2.20.33"></a>

非常直觀簡潔，照著這樣的寫法，很快就可以做出一個精美的後台。

看上它的第一點原本是因為豐富的 field options。但後來開始越翻越深，才發現這套後台把 block + form builder 玩的出神入化。而另外更神奇的是，它還整合了常用的幾組 plugin：<a href="https://github.com/plataformatec/devise/wiki">devise</a>, <a href="https://github.com/thoughtbot/paperclip">paperclip</a>, <a href="https://github.com/ryanb/cancan/wiki">cancan</a>。做到很多必須要自己刻的 feature。但在架構上又分離的很乾淨。

也實作了很多 model association 才會用的 user interface（has_many, has_one, belongs_to )。當然你也可以把 form 抽換成自己 form builder 和插入自己寫的 partial 達到整合的效果....不只如此，還可綁 CKEditor，犯規中的犯規。 

文件寫的還只是這些 feature 的一小部分，如果你熟 Rails 的話，直接看 source code 還可以挖出更多的可能性。簡直威到不能再威。

開始認真考慮以後只要是後台都用 Rails Admin 去客製了 ...XD


====
<blockquote>

廣告：
<a href="http://rails-101.logdown.com/">書：第一次學 Rails 就上手</a> - 7 天之內學會 Rails（USD $9.99）
<a href="http://registrano.com/group/rubytaiwan">聚會：Rails Tuesday</a> Rails 社群聚會 （免費）2011/06/28 這次換成行天宮捷運站不同的出口了，請注意喔！
</blockquote>

