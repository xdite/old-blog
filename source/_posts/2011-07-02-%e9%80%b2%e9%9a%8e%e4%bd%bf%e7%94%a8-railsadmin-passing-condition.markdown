--- 
wordpress_id: 2591
layout: post
title: "\xE9\x80\xB2\xE9\x9A\x8E\xE4\xBD\xBF\xE7\x94\xA8 RailsAdmin : passing condition"
date: 2011-07-02 21:06:08 +08:00
wordpress_url: http://blog.xdite.net/?p=2591
---
在<a href="http://blog.xdite.net/?p=2559">前一篇文章</a>提過，RailsAdmin 可以拿來建構簡易的 Admin 後台。

但如果拿 RailsAdmin 來建構「給使用者」用的後台呢？

比如說，「每個使用者進後台，都只能列出並編輯自己的文章」？

翻翻原始碼，看起來好像很難做到....因為 RailsAdmin 先天上無法 pass scope 和 condition 進去。

但很難並非無法做到。週末在翻修 <a href="http://logdown.org">logdown.org</a> 的後台，便想使用 RailsAdmin 去改善後台，其中有個「feature」就是「每個使用者進後台，都只能列出並編輯自己的文章」。

翻了好一陣子文章和討論串，終於找到 <a href="https://github.com/sferik/rails_admin/pull/301">ryanb 提供的一個解法，利用 cancan 解決</a>。

做到以下效果

<a href="http://www.flickr.com/photos/xdite/5893927412/" title="螢幕快照 2011-07-02 下午8.59.23 by xdite, on Flickr"><img src="http://farm6.static.flickr.com/5306/5893927412_fa0c1890d4.jpg" width="500" height="341" alt="螢幕快照 2011-07-02 下午8.59.23"></a>

當然這個 pull request 大家看完可能還會摸不著頭腦。提供我的 ability.rb 給大家做參考

<script src="https://gist.github.com/1060066.js?file=ability.rb"></script>

===
			<blockquote>

廣告：
<a href="http://rails-101.logdown.com/">書：第一次學 Rails 就上手</a> - 7 天之內學會 Rails（USD $9.99）
<a href="http://registrano.com/group/rubytaiwan">聚會：Rails Tuesday</a> Rails 社群聚會 （免費）
</blockquote>
		
