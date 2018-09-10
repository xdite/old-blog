--- 
wordpress_id: 3179
layout: post
title: !binary |
  W0Fzc2V0IFBpcGVsaW5lXSDomZXnkIYgdmVuZG9yIOmhniBqcyBwbHVnaW4g
  5aaCIFRpbnlNQ0Ug55qE5ZWP6aGM

date: 2011-09-28 17:07:38 +08:00
wordpress_url: http://blog.xdite.net/?p=3179
---
一般來說，簡單的 js plugin 還蠻好掛上 Rails 3.1 的 projects。但是複雜如 <a href="http://www.tinymce.com/">TinyMCE </a> 這種套件呢？

相信我，一定可以把你搞到快往生。

因為 TinyMCE 是採用動態載入的形式，這也使 asset pipeline 提供的 precompile + fingerprinting feature 與它格格不入。

而一般的 TinyMCE 資料夾底下通常會放置了很多 plugins 與 themes，這也使得 assets pipeline 在 load assets 時格外困難。（你總不想很蠢的直接在 production.rb 裡面一個一個指名路徑吧。）

解法是：

安裝 <a href="https://github.com/spohlenz/digestion">digestion</a>

然後在 Gemfile 下 enable 它

<pre>
gem 'digestion'
</pre>

* 請注意，別把它扔進 Gemfile 裡的 assets group 裡，這會使得這個 gem 在 production 一點作用也沒有。

然後在 production.rb 裡加入這兩行。

<pre>
    config.assets.digest_exclusions << "tiny_mce/*"
    config.assets.precompile << "tiny_mce/*"
</pre>

就能解決你的問題了...

===

P.S. 若你很懶惰的話，也可以直接安裝 <a href="https://github.com/spohlenz/tinymce-rails">tinymce-rails</a> 這個 gem（感謝 <a href="http://twitter.com/ihower">@ihower</a>提供 ，原理類似。只不過我們的狀況有太多 customized plugin，必須用 digestion 的方法繞過。
