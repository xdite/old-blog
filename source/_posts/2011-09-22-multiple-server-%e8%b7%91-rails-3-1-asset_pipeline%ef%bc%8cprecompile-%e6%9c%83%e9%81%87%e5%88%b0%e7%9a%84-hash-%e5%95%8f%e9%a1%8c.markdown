--- 
wordpress_id: 3128
layout: post
title: "multiple server \xE8\xB7\x91 Rails 3.1 asset pipeline\xEF\xBC\x8Cprecompile \xE6\x9C\x83\xE9\x81\x87\xE5\x88\xB0\xE7\x9A\x84 hash \xE5\x95\x8F\xE9\xA1\x8C"
date: 2011-09-22 22:47:06 +08:00
wordpress_url: http://blog.xdite.net/?p=3128
---
今天終於讓 <a href="http://t17.techbang.com.tw">T17</a> 正式 deploy Rails 3.1 到 production 了。中間也解了不少問題。

不過最難纏的當數今天 deploy 到正式站，遇見的這個問題。

我們有兩台以上的機器做 HA，但兩台的 production 壓出來的 asset 的 digest url 卻不一樣。

造成我們的 CDN cache asset 時會有問題，有點類似舊文「<a href="http://blog.xdite.net/?p=2452">Rails Project 在 Load Balancer + CDN 時會遇到的問題</a>」會撞見的 asset 脫光光問題。

本來懷疑是 deploy precompile asset 的順序問題，後來調整了也沒用。找了半天換手請同事 @vincent 找，也研究了不久才抓到罪魁禍首：

<pre>

#header {
   position: relative;
   z-index: 99999;
-  background: image-url("/assets/images/header-background.png") ;
+  background: url("/assets/images/header-background.png");

</pre>

這一段在 SCSS 裡面作用是類似的。但在多台時，image-url 產生的 png 路徑與 url 產生的路徑卻會不同。image-url 產生出來的路徑是動態的...

而 Rails 的 digest url 路徑 是根據 hash css 內容所算出來的，因此不同兩台機器，很有可能就產生出不同的 url。因此 CDN 就會吃到白的 CSS (404) 了...

解決的方式就是<strong>把有用到 image-url 的部份通通改用 url 就可以了</strong>。

後來得知是 helper 問題後，把 image-url 當關鍵字丟進去 google 找，也發現<a href="https://github.com/rails/sass-rails/issues/45">有人已經在 sass-rails 這個專案哀號過</a>了 XD。
