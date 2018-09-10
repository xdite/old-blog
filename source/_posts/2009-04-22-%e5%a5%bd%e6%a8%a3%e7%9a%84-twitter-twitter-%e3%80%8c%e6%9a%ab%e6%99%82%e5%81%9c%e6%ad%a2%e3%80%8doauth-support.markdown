--- 
wordpress_id: 1172
layout: post
title: !binary |
  5aW95qij55qEIFR3aXR0ZXIgLSBUd2l0dGVyIOOAjOaaq+aZguWBnOatouOA
  jU9hdXRoIHN1cHBvcnQ=

date: 2009-04-22 22:04:50 +08:00
wordpress_url: http://blog.xdite.net/?p=1172
---
update : <a href="http://news.cnet.com/8301-13577_3-10225103-36.html">根據 CNET 的報導</a>，Yahoo 也在接近的時間，停止了 Oauth support。<strong>原因是 Security Issue。期望在這幾天之內會解決</strong>。

大概在四五個小時之前，根據 TC 的報導，<a href="http://www.techcrunch.com/2009/04/22/twitter-oauth-temporarily-disabled-leaves-developers-hanging/?awesm=tcrn.ch_VV&utm_medium=awesm-twitter&utm_content=techcrunch-autopost&utm_campaign=techcrunch&utm_source=twitter.com">Twitter 無預警宣布的宣布「暫時停止」Oauth support</a>。

事實上，如果您是我開發的書籤網站「<a href="http://twitio.us">Twitio.us</a>」的用戶的話，登入時也會看見這張圖片。

<a href="http://www.flickr.com/photos/xdite/3465795410/" title="Flickr 上 xdite 的 6323624"><img src="http://farm4.static.flickr.com/3602/3465795410_4c04f11053_o.png" width="553" height="344" alt="6323624" /></a>

一整個莫名其妙。還好我用 Rails 的 db migrate 加 git ，把資料庫和程式 rollback 到改用 Oauth 之前的程式版本<strong>使用「傳統認證</strong>」，不然豈不直接就死在沙灘上。<del datetime="2009-04-22T21:11:13+00:00">Twitter 做事態度一整個誇張</del>....

Anyway 請安心繼續使用。這次的不便是 Twitter 帶來的，不是我 ...-_-
