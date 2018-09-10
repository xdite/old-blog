--- 
wordpress_id: 2109
layout: post
title: "Ruby implementation of Reddit ranking algorithm "
date: 2011-03-21 02:04:42 +08:00
wordpress_url: http://blog.xdite.net/?p=2109
---
最近在寫新產品，需要熱門討論的演算法，之前想了幾個架構都不合用，在找資料時是有翻到 <a href="http://www.google.com.tw/url?sa=t&source=web&cd=1&ved=0CCgQFjAA&url=http%3A%2F%2Fwww.reddit.com%2F&ei=L0GGTc2fL4KmvgOl3OXFCA&usg=AFQjCNGP2I23xwAzzicm-TwmDMKVU72m0w&sig2=rPQm_wnTpQUe6rypA_eWSQ">reddit</a> <a href="http://code.reddit.com/browser/r2/r2/lib/db/_sorts.pyx">將他們的 ranking algorithm opensource 出來</a> ( pyrex 版本）。

後來 plurk.com 的 <a href="http://amix.dk/blog/post/19588">amix 又把它改寫成純 python 版本</a>。前一陣子開了一張 ticket 提醒自己找時間重寫成 Ruby 版本。

剛剛在 stackoverflow 亂翻東西時，發現<a href="http://stackoverflow.com/questions/5365525/why-is-my-rank-sum-database-column-still-nil">有人昨天把它寫出來</a>了 XDDDDDDD （真巧）

不過我只取推文數做分數而已...

<script src="https://gist.github.com/878503.js?file=topic.rb"></script>
 
update : stackoverflow 也有公開自己的 <a href="http://meta.stackoverflow.com/questions/11602/what-formula-should-be-used-to-determine-hot-questions">algorithm</a> 
