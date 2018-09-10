--- 
wordpress_id: 976
layout: post
title: Unofficial Plurk API in Ruby
date: 2009-03-01 04:24:14 +08:00
wordpress_url: http://blog.xdite.net/?p=976
---
最近 <a href="http://plurk.com">Plurk</a> 似乎很夯。

去年底剛開始玩 plurk 時，有嘗試想為它寫 Ruby Library（因為沒人寫），但是才寫幾個 method 就半途而廢了，一直擺著。最近 Plurk 很夯，加上大家對 Karma 的升降真是又愛又恨。有幾個朋友，就開始盧我寫 Plurk Karma 代練機器人。結果<a href="http://twitter.com/xdite/statuses/1223011212">才在 Twitter 上面嘴砲說要做個代練網站。</a>沒想到 <a href="http://twitter.com/evenwu">Evenwu</a> 看到，就真的做了一個<a href="http://evendesign.tw/plurk">代練價目表</a>出來，囧。

最後想想，就算不是要拿出來賣，自己掛也好。就開始把塵封的那一段 Library 拿來改，可是左看右看，覺得當初寫的程式碼實在太噁心了，完全不能看。於是就發狠整個打掉重寫 ....

剛剛終於寫好了，我把成果放在 Github 上：

<big> <big> <strong> <a href="http://github.com/xdite/plurk/tree/master">http://github.com/xdite/plurk/tree/master</a></strong></big></big>

需要的人請自行取用。如果覺得我寫的程式很難看，歡迎幹角我並把 patch submit 回來（你必須先 submit 才能幹角我XD)，我會 pull 回 master。

-----
寫到最後，看到只要需要 regex ，我就會反胃...orz。

稍微寫一下開發心得好了，Plurk 因為正在衝會員成長與 PV 數，不希望像 Twitter 一樣，到最後覺大多數的 Request 都來自 API Request。所以並沒有官方 API，目前的 <a href="http://search.cpan.org/dist/WWW-Plurk/">Perl </a>/ <a href="http://code.google.com/p/plurkapipy/">Python</a> / <a href="http://code.google.com/p/rlplurkapi/">PHP</a> / <a href="http://code.google.com/p/plurkapi/">C#</a> 包括我寫的 <a href="http://github.com/xdite/plurk">Ruby Library</a> 都是連過去 HTML 頁面丟 GET / POST 再用 regex 處理 javascript 硬幹出來的。所以開發者必須一直跟 regex 打交道。

還有網頁上並不是純 JSON，所以必須自己寫 regex 自己轉成 JSON，再轉成自己要的 Array 或 Hash 。總之一切都是硬幹到底...硬幹魂啊 T_T
