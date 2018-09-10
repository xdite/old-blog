--- 
wordpress_id: 1159
layout: post
title: "Sinatra : Ruby Lightweight Web Framework"
date: 2009-04-21 13:42:39 +08:00
wordpress_url: http://blog.xdite.net/?p=1159
---
<a href="http://osdc.tw">OSDC 2009</a> 上我請 itsZero 在他的 lightning talk 中，幫忙發表了 <a href="http://blog.xdite.net/?p=1152">Sinatra on the Cloud</a>。

礙於那一場是 lightning talk，我們沒有花時間著墨在介紹 <a href="http://www.sinatrarb.com/">Sinatra</a>，也沒有著墨如何利用 Sinatra 開發應用，只專注發表 Sinatra 可以做出的應用。

所以這一篇就是我認真來介紹 Sinatra 是什麼，可以拿 Sinatra 來做什麼以及如何拿 Sinatra 做事的文章。

如同標題所說，Sinatra 是一個輕量級的 lightning Web Framework。

Sinatra 與 Rails 主要的差別在於 Sinatra 並不是 MVC 式的 Framework。Sinatra is made of a minimal  DSL-syntax。它的 core 並不包含類似 ActiveRecord ( Rails 的 ORM ) 這樣的 module。通過使用 get/post action 定義，Sinatra 具備了 dynamic route definition。

使用 Sinatra 的目的並<strong>不是用來開發那些巨型的 Web application ，而是搭造那些小型的應用程式或者API介面</strong>。這樣做有些什麼好處呢? 如果你<strong>只是想寫一支小型程式，或者開發 API，並不需要使用 Rails 這麼複雜(或肥)的 Framework做這些簡單的事。使用 Sinatra 既簡單，Respond 也迅速</strong>。

在前面幾篇 post 中，我提及了 Rails 2.3 的新特性：<a href="http://blog.xdite.net/?p=1133">Template 以及 Engine</a>。

Rails 2.3 除了前述兩大武器之外，還引進了 Rack 以及 Rails Metal。Metal 的出世的意義在於，<strong>Rails 可以引進同樣使用 Rack middleware 的 web framework 開發的程式，替換掉 Rails 程式裡面原有的 action</strong>。嫌用 Rails 開發 API 耗費資源嗎? 嫌用 Rails 寫上傳功能，結果一個人上傳堵住整個站也堵住嗎? <strong>有了 Rails Metal，你可以用 merb 或 sinatra 寫好這些功能，然後利用 Metal 掛上去</strong>。提昇效率也減少資源的耗用。

提昇的程度有多高呢? 來看看<a href="http://www.javaeye.com/topic/349203">這一篇文章提供的 ab 數據</a>。

<blockquote>在 （Ubuntu 8.04, Intel Atom N270 @1.60GHz） 的 production運作。

單獨 ab 由 rails controller 寫的 hello world

Requests per second:    81.09 [#/sec] (mean)
Time per request:       12.332 [ms] (mean) 

ab 藉由 掛起來的 sinatra  （替換掉原有 action 後速度明顯的提升）

Requests per second:    163.75 [#/sec] (mean)
Time per request:       6.107 [ms] (mean) 

單獨的 sinatra （當然單獨還是比較猛）

Requests per second:    450.56 [#/sec] (mean)
Time per request:       2.219 [ms] (mean)

</blockquote>

除此之外，使用 Sinatra 開發網頁應用，需要的程式碼也相當精簡。比如說我和 itsZero 所發表的程式，都是只有 50 行的 Application。因為沒有綁 ORM，換句話說就是後面愛接什麼 ORM （ActiveRecord、DataMapper）或 DB 都可都可以。

只要熟 gem 和 library 的引用，便可以用更少（少上許多）的程式碼做出相同的 Application。比如說我的 <a href="http://github.com/xdite/twitter-message-wall/tree/master">twitter message wall</a>。就是只用 library（mixed and hacked ）和 gem 搭出來的。沒有 db 的原型版本，只花我 10 分鐘寫了 10 行就搞定...。

而如果你又是 Ruby 上癮者，想在 <a href="http://appengine.google.com">GAE</a> 上開發程式，卻打死都不想學 python 和 JAVA。<a href="http://blog.bigcurl.de/2009/04/running-sinatra-apps-on-google.html">Jruby with Sinatra</a> 更是相當讚的好選擇。

（ 我寫了一個 <a href="http://github.com/xdite/twitter-message-wall-gae/tree/master">Jruby-Sinatra-GAE-Bigtable-Will_paginate</a> 的 demo，之後會找時間講解）。

Anyway，如果你是 Ruby / Rails Developer ，抑或只是想嘗試用少許 code 就做出多多事的開發者，就快來試試看 Sinatra 吧....。

延伸閱讀：<a href="http://www.ruby-yee.com/2008/12/28/sinatra-29-links-and-resources-for-a-quicker-easier-way-to-build-webapps">Sinatra: 讓你更快, 更容易地建立Web程序的29個鏈接和資源</a>
延伸教材：<a href="http://www.pragprog.com/screencasts/v-aksinatra/classy-web-development-with-sinatra">Classy Web Development with Sinatra</a>
