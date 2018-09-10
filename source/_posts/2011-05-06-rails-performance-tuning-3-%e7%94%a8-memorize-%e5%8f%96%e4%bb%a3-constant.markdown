--- 
wordpress_id: 2249
layout: post
title: "Rails Performance Tuning (3) : \xE7\x94\xA8 memorize \xE5\x8F\x96\xE4\xBB\xA3 constant"
date: 2011-05-06 14:29:00 +08:00
wordpress_url: http://blog.xdite.net/?p=2249
---
在<a href="http://blog.xdite.net/?p=2241">上一個 tips </a>裡，我們提到了在 view 裡面做 query 再 cache 是實作 sideabar 和動態 menu 一個比較好的方式。

於是接著自然就會開始想，如果要節省資源，是不是要把昂貴或常用的資料塞進去 model 裡的 CONSTANT 裡，這樣比較省。

<a href="http://www.flickr.com/photos/xdite/5692669340/" title="螢幕快照 2011-05-06 下午2.13.11 by xdite, on Flickr"><img src="http://farm6.static.flickr.com/5182/5692669340_da6998f94c.jpg" width="500" height="84" alt="螢幕快照 2011-05-06 下午2.13.11"></a>

"YES" & "NO"

Why "YES"?

<blockquote><strong>CONSTANT 的確會幫你把 query cache 起來。</strong></blockquote>

Why "NO"?

但是會發生當初沒預料到的情形：

<blockquote>(1) 在 view 裡面去拉 model 裡的 CONSTANT，很抱歉，資料不會拉記憶體 cache 住的，它還是會從資料庫重拉，效果就沒有了。
(2 ) 因為他是 model 的 CONSTANT，這表示你拉下這一個 model 的任何 object，都會去順帶去拉這個 expensive query ......（雖然已被 cache 起來）XD
(3) 如果你在 view 裡面直接拉這個 model 的 object ，綜合 (1) 與 (2) 的效果，那就很歡樂啦 XD</blockquote>

那要怎麼解決？

<blockquote>

Well，你可以使用 <a href="http://apidock.com/rails/ActiveSupport/memoize">ActiveSupport 的 memoize 這個 feature</a>，把 expansive query 包成 method，再 memoize 起來，取代原先的 CONSTANT 作法。
</blockquote>
