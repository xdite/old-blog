--- 
wordpress_id: 1419
layout: post
title: !binary |
  5ZyoIFNub3cgTGVvcGFyZCDnt6jora/lpZfku7bnmoTms6jmhI/kuovpoIU=

date: 2009-08-28 18:38:15 +08:00
wordpress_url: http://blog.xdite.net/?p=1419
---
Snow Leopard（ MacOS 10.6）出來了，這次更新些甚麼我就不多說了（請自行 Google 官方的 Release Note）。

不過因為我神經太大條，搶先升級，不幸英勇陣亡（不怕死的在開發機升級）了 。所以在這裡稍微提醒一下後面想升級的 Devloper 一些升級以後會遇到的雷....



<blockquote>1. 不是所有原先的軟體都能跑在 Snow Leopard 上，比如說 PlugSuit Agent ...。

2. /usr/local/ 下裝的東西會神祕的消失，於是我的 mysql 就不幸的也死了..還好裡面都是測試資料。

3. 當我發現 mysql 不幸消失之後，打算用 port install 重裝 mysql，結果 macports 也爛了。
    <a href="http://forums.macrumors.com/showthread.php?t=720035">解法是去拉 trunk 版的，自己編譯安裝</a>...:/

4. 安裝 mysql 的時候，發現 readline 之類的一些也要順便 upgrade，就順手 upgrade 了。
    結果我開 rails console 時，因為牽扯到 readline，console 就死掉了...
    直覺解法就是重編安裝我的 ruby（Ruby Enterprise Edition )。
    然後在 make 時候也爛了...

    還好昨天在裝 sphinx 時，也遇到 make 不過的問題。跟燈哥稍微討論了一下，才發現是 <strong>*Snow Leopard 的 GCC profile 有改，預設 arch 是 x86_64*</strong>。解法當然就是用 <a href="http://blog.d27n.com/2009/08/26/mac-os-x-snow-leopard-rails-mysql-and-sphinx/">32-bit mode compile</a>。

ruby 也順利的循此法重裝好了...

5. 因為 ruby 重裝了，所以換我的 rmagick 爛了。

rmagick 是按照<a href="http://www.agileanimal.com/2009/08/11/imagemagick-and-rmagick-on-leopard-and-other-large-white-cats">這一篇</a>裝的，但是會遇上 ImageMagick make 爆炸的問題。所以必須把 script 中 ./configure 那一行取代成
<a href="http://gist.github.com/177031">這個 gist 建議的參數</a>。

這樣才能順利 make & make install。（我一直不停的換參數測試，重編了不下十次才成功...幹）

6. mysql 最後是裝 mysql.com 提供的 mpkg，而非手動編。因為 mysql 重裝了，mysql ruby gem 也要重裝，很不幸的 rubyforge 只有 mysql 2.8.1，但這版本是有問題的，會跟 rails 相衝（跑有關 db 的 rake command 會出現 <strong>NameError: uninitialized constant MysqlCompat::MysqlRes</strong>）。2.7.x 版的沒有這個問題，但很遺憾的 rubyforge 拿掉了 2.8.x 以下的 gem，只能抓 tgz 手動編。這時候又會撞上鬼打牆的 32/64 bit 問題，最終解法是如果你是 snow leopard 的用戶，mysql 必須裝 x86_64 版 mpkg，2.7 才能順利的編上去。

</blockquote>



到目前為止，目前 Rails 的環境算是可以順利的跑了，但是我非常想殺人.....

<big><big><strong>
如果你是 Rails Developer ，我會強烈建議不要升 10.6 ...</strong></big></big>


有什麼地雷我會繼續上來靠北和更新解法的。

( 抱頭懺悔....orz)
