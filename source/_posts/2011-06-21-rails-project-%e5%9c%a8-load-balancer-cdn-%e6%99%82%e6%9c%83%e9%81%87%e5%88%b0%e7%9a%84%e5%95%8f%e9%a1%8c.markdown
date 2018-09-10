--- 
wordpress_id: 2452
layout: post
title: !binary |
  UmFpbHMgUHJvamVjdCDlnKggTG9hZCBCYWxhbmNlciArIENETiDmmYLmnIPp
  gYfliLDnmoTllY/poYw=

date: 2011-06-21 18:29:57 +08:00
wordpress_url: http://blog.xdite.net/?p=2452
---
公司最近進了一台硬體 Load Balancer，最近開始在把 project 通通丟到 load balancer 後面。其實之前就有用過 HAProxy 先嘗試擋在<a href="http://t17.techbang.com.tw"> T17 討論區</a>上面。但是那時候遇到一個靈異問題，剛 deploy 完網站衣服都會脫光光，而且機率忽大忽小，是個不知如何重製起的 bug。

只知道單台機器就不會有這樣的問題。後來因為機器不太夠用的問題，又撤掉了 HAProxy，這問題就沒再被提起。

最近比較有空要把超貴的 LB 跑起來了，這個問題又被拿出來討論。我們家的 SA vincent 研究一整個下午終於抓出來是 deploy 時間差問題。假設我們把一個站放到 A、B、C 三台機器上，假設今天剛 deploy 完。而使用者連到的是 A 機器，吃了 A 機器的 html + CDN 上的 CSS。這時候因為剛 deploy 完，CDN 一定會跟源頭要檔案，但 CDN 可能連到還沒有 deploy 好的 B 機器，B 機器沒有這個壓好的 css 檔案 會噴<strong> error ( error is not the same as 404 )</strong>。於是頁面就被脫光光。

由於 Rails 的 all.css 這一類，是第一個使用者連進來才壓制，所以單台機器 + CDN 就不會有這個問題。而兩台以上就爛光光了....

vincent 想的解法是把 CSS 拆成 partial，然後先丟到一個空頁面，deploy 時 wget local 硬是先壓出來一個出來。但是這解法看起來就相當不蘇胡...

而且也沒有「完全」解到時間差問題。只是能壓低機率而已。

下午就幫忙想。還打電話問了別公司的幾個 SA，不過得到的解法都不太適用我們的架構 :/ 本來想說就先放著。我當時能想到的解法還是只能去翻 source code，然後寫支 rake，deploy 後重開前 自己先 precompile。

後來因為今天<a href="http://www.techbang.com.tw"> T 客邦</a>四個站要上新 header，把 header 高度全砍成原先的 2/3。不料 deploy 下去全爛光，主要原因是因為 background image 全被 CDN cache 住了。Rails 雖然 css 這一類的 asset 可下 query string 來解決 invalid 問題，但是 css 裡面的 url 就沒那麼幸運了。都吃舊圖，header 爛一片....

因為我們全部的站都上了 Comass ( SCSS ) ，本來想說要用 Compass 的 asset_cache_buster 去解決，也塞 query string 在 css 的 image 後面，後來發現 asset_cache_buster 是爛的不會動，而這個 bug 「<a href="https://github.com/chriseppstein/compass/issues/434">昨天才剛被解掉</a>」。太酷了... =_= 

既然不確定有沒有上 beta，而上了 beta 又不確定會不會炸掉。於是就放棄了走這條路。

幾分鐘之後我又想起了有一套 Gem 「<a href="http://documentcloud.github.com/jammit/">Jammit</a>」似乎好像也有類似功能，就翻出來試試，反正用搭的今天也要硬解掉這個問題。不出我所料，摸了幾分鐘做實驗就解了塞 query string + timestamp 的需求。

<a href="http://www.flickr.com/photos/xdite/5855855563/" title="螢幕快照 2011-06-21 下午6.28.29 by xdite, on Flickr"><img src="http://farm3.static.flickr.com/2591/5855855563_df3e06af24.jpg" width="500" height="145" alt="螢幕快照 2011-06-21 下午6.28.29"></a>

<script src="https://gist.github.com/1037580.js?file=gistfile1.txt"></script>

因為 document 很長，我沒耐性看完。但心想既然有辦法 Jammit 內建功能那麼多，有打包、壓縮、Cache busting。應該也有 Precompile 的選項吧，就叫 vincent 也翻翻看...果不其然真的有。

<strong>Jammit.package!</strong> 然後塞進 Capistrano recipe 就解了..

<script src="https://gist.github.com/1037574.js?file=gistfile1.txt"></script>

今天真是幸運日啊...

====
<blockquote>

廣告：
<a href="http://rails-101.logdown.com/">書：第一次學 Rails 就上手</a> - 7 天之內學會 Rails（付費）
<a href="http://registrano.com/events/f56f33">課程：RoR 開發環境安裝教學</a> - 打造 bug free Rails 開發環境（付費） - 2011/07/03
<a href="http://registrano.com/group/rubytaiwan">聚會：Rails Tuesday</a> Rails 社群聚會 （免費）2011/06/21
</blockquote>
