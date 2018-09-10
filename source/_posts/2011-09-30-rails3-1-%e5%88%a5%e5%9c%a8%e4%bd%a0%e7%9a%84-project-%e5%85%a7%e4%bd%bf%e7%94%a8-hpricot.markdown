--- 
wordpress_id: 3236
layout: post
title: !binary |
  W1JhaWxzMy4xXSDliKXlnKjkvaDnmoQgcHJvamVjdCDlhafkvb/nlKggaHBy
  aWNvdA==

date: 2011-09-30 15:59:58 +08:00
wordpress_url: http://blog.xdite.net/?p=3236
---
這是這禮拜最難抓到的一個問題，因為無跡可尋。在升級 T 客邦的過程中，我撞到一次過，有在 redmine 上記載下來。結果同事 @vincent 還是在另外一個站台升級時又繼續撞了一次。

它的錯誤訊息只有這樣：

<pre>

ArgumentError: wrong number of arguments (1 for 0)
    from /Users/username/.rvm/gems/ruby-1.8.7-p352@pah-rails3/gems/builder-3.0.0/lib/builder/xmlbase.rb:135:in `to_xs'
# 略...
</pre>

在啟動 Rails 時或跑 Rake 時，一 initialize 就會出現，trace code 你完全不知道到底到底誰跟 builder 撞了...

<h2> Hpricot </h2>

用到 xml 的嫌疑犯很多，後來一個一個移掉檢查，發現是 <a href="https://github.com/hpricot/hpricot">hpricot</a> 惹的禍。它自己定義了 to_xs 與 builder 撞到。


從 <a href="https://github.com/rails/rails/issues/2813">Rails 上的這個 issue 上</a> 也 confirm 了這個猜想，Rails core team 成員 @tenderlove 的回應是「你應該去跟 hpricot 靠北，這是 freedom patches 的代價」然後就把這張票關掉了 XD。

所以現在<a href="https://github.com/hpricot/hpricot/issues/53">就剩下這張 ticket 還活在 hpricot</a> 上。不過 hpricot 超久沒更新，我想短期改善的希望也很渺茫吧....
