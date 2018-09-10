--- 
wordpress_id: 1839
layout: post
title: "\xE5\x9C\xA8 Rails3 \xE4\xB8\x8A\xE4\xBD\xBF\xE7\x94\xA8\xE6\xBC\x82\xE4\xBA\xAE\xE7\x9A\x84 console ( ~/.irbrc compatible with rails3)"
date: 2010-08-23 13:49:01 +08:00
wordpress_url: http://blog.xdite.net/?p=1839
---
在開發 Rails Application 時，蠻多機會會使用到 script/console 去做一些 command 的操作。但是這個 console 其實十分單調，也不方便。

往往會有以下麻煩事 :



<blockquote>1. 必須開一個 script/console, 一個 script/server。才能知道在 console 下，哪一些指令生成了怎樣的 db query ...
2. 從 script/console 拉出來的 db object，呈現出來的是一串擠在一起的 Hash，非常難以閱讀。</blockquote>



所以有人就調製了一個精心的 ~/.irbrc <a href="http://www.tech-angels.fr/post/963080350/improve-irb-and-fix-it-on-mac-os-x">Improve IRB (and fix it on mac OS X)</a>

效果如下：<strong>同時拉出彩色版 log 以及對 db object 做排版</strong>。（這對開發上極有幫助，同時我也調了 ~/.irbrc,  <a href="http://blog.xdite.net/?p=1822">加上 pasteboaRb</a>，讓我的 console 更變態）。

<a href="http://www.flickr.com/photos/xdite/4918578029/" title="Flickr 上 xdite 的 Rails3 console with ~/.irbrc"><img src="http://farm5.static.flickr.com/4114/4918578029_bdd22b3e9c.jpg" width="500" height="170" alt="Rails3 console with ~/.irbrc" /></a>

但是問題來了。上面的那個版本 ~/.irbrc 在 Rails3 是不管用的。因為你會遇到 :

1. bundler 會比你的 irbrc 早 load 進來，於是他會一直該找不到 gems : wirble, hirb ...etc.。解決方法是去 Gemfile 寫 gem "wirble"，gem "hirb" ...etc. 但是這實在是很不必要的一件事。只要專案中有一個人擴充他的 irbrc 裡面的 gem set 就會....!@#$%^。

研究出來比較漂亮的解法就是，改 project 下的 script/rails ，在前面加上：

<script src="http://gist.github.com/544867.js?file=.irbrc%20compatible%20with%20rails3"></script>

搶在 bundler 之前 load 進來就贏了...

2. 解決 require 的問題之後，接下來會面對的問題是「彩色」server log 不見了。/_\ 網路上找到的幾個 ~/.irbrc compatible with Rails3 solution 都沒有用。最後終於研究出來解法。在 config/environments/development.rb 裡最下面放進這兩行，彩色 log 就回來了！

<script src="http://gist.github.com/544873.js?file=development.rb%20for%20show%20log%20in%20rails3%20console"></script>

請好好享用吧 :D
