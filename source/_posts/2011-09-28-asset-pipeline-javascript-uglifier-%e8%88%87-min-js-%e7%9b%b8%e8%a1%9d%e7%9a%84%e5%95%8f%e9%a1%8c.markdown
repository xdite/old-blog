--- 
wordpress_id: 3188
layout: post
title: "[Asset Pipeline] javascript uglifier \xE8\x88\x87 min.js \xE7\x9B\xB8\xE8\xA1\x9D\xE7\x9A\x84\xE5\x95\x8F\xE9\xA1\x8C"
date: 2011-09-28 17:20:46 +08:00
wordpress_url: http://blog.xdite.net/?p=3188
---
這次的 Asset Pipeline 提供了 javascript uglifier 這項 feature，不只是壓縮而已，它也幫忙了將變數 uglify 掉。

不過這個功能實際使用時，我們發現一個問題。uglifier 提供的壓縮變數也是使用 a,b,c,d,e,f,g 等規則。

這會造成...

將 jquery.min , jquery-ui.min 大包的 library 丟進去與其他 js 一起壓時，壓出來的 js 是異常的。( xx is undefined )

因此有一些 function 就此爛掉...

解決方式：

<blockquote>
別把 jquery 丟進去一起壓，使用 Google 提供的 jquery 另外載入。

</blockquote>

至於自己手寫的才使用 uglifier 壓縮打包。

