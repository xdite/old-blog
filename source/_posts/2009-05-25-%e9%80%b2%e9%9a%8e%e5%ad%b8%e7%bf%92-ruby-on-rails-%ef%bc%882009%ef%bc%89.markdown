--- 
wordpress_id: 1227
layout: post
title: !binary |
  6YCy6ZqO5a2457+SIFJ1Ynkgb24gUmFpbHMg77yIMjAwOe+8iQ==

date: 2009-05-25 20:46:43 +08:00
wordpress_url: http://blog.xdite.net/?p=1227
---
<a href="http://blog.xdite.net/?p=1190">上一篇</a>提及了一些入門必練的基礎。而現在要繼續寫的是進階篇。大致分為兩個方向：隨心所欲整合 / Scale 與 Deploy

<strong>* 隨心所欲的整合</strong>



<blockquote>1. 認識更多的 Plugin / Gem，減少重造輪子的機會
Gem / Plugin 的擴充性一向對 Rails 的開發加速作用有大大的加分效果。
- <a href="http://github.com">Github</a> 或 <a href="http://rubyforge.org">RubyForge</a>
- <a href="http://railsenvy.com">RailsEnvy</a> 每週介紹 新穎 / 亮眼 / 實用 / whatever 的 gem / plugin 與新知

2. 可以移植到其他專案的好用 code，重寫成 plugin 或者 porting 成 <a href="http://blog.xdite.net/?p=1126">Rails Engine</a>。撰寫自己的 <a href="http://blog.xdite.net/?p=1133">Template</a>。
<a href="https://peepcode.com/products/rails-2-plugin-patterns">Plugin Patterns (Peepcode)</a> 想學會寫 Rails plugin 請看這份 PDF 。
Rails Engine # 把以前的 app 當成一個 plugin 使用
Rails Template # 每次開專案都會安裝的套件以及執行的動作寫成 Template

3. 開發自己的 View Helper 以及 Form builder。
Rails 不僅僅提供好用的 helper，甚至你可以利用 Rails 提供的 API 撰寫自己的 Helper 和 Builder。為自己的專案套上 standard layout 以及減少寫噁心 html 的機會。
 <a href="http://github.com/ihower/handicraft-interfaces/tree/master">handicaft_interfaces</a>
 <a href="http://github.com/ihower/handicraft_ujs/tree/master">handicraft_ujs</a>
 <a href="http://github.com/ihower/handicraft_helper/tree/master">handicraft_helper</a>

建議閱讀 ihower 在上次 Ruby Tuesday 釋出的 <a href="http://www.slideshare.net/ihower/building-web-interface-on-rails?type=presentation">Building Web Interface on Rails</a>，以及把玩 ihower 在 github 上提供的 demo application。這次的 demo 含了 Rails3 提供的 Unobtrusive Javascript 的概念實作，使用 jQuery on Rails 作為範例。

4. 對於各項第三方整合方案的熟悉 

Rails 只是一套網頁框架，但它並不是阿拉丁神燈。比如說你需要搜尋功能、付款功能、寄送簡訊、與 IM 整合 ...。Rails 當然 ....<strong>*不可能*</strong> 內建。雖然比較少書籍在介紹這方面的資訊，但是有幾個 Site 出了不錯的 Video Turtorial（付費 / 不付費都有..)。

<a href="http://peepcode.com">Peepcode</a>  
<a href="http://railscasts.com/">Railscasts</a>
<a href="http://asciicasts.com">AsciiCasts</a> # Railscasts 的圖文教學版 ...

<a href="http://railskits.com/">Railskits</a> # 當然你有能力開發、有能力改，但沒時間從頭開發，也可以從這裡買 Solution 回去改 ...

5. Deep in Ruby / Rails
<a href="http://www.oreilly.com/catalog/9780596510329/index.html">Advanced Rails (O’Reilly)</a> 難得真的有 Advanced 到的書
Writing Efficient Ruby Code (Addison Wesley shortcut PDF) 
Code Review (Peepcode)</blockquote>


<strong>
* Scale 與 Deploy</strong>

當站大了（code 變多, query 變多 , 活動變多 ) 以後就會遇到架構複雜 / 速度慢的問題。有幾個方向是可以鑽研的



<blockquote>1. 換掉 Ruby 。
well ... Ruby 有很多種版本，也許你可以嘗試換上 <a href="http://www.rubyenterpriseedition.com/">Ruby Enterprise</a> 節省記憶體以及改善 GC 。

2. 嘗試不同的 Web Server / Rails Web server，找出最適合的搭配。
Mongrel / Thin / mod_rails / FastCGI +  Apache / Nginx / lighthttpd 都是可以嘗試的組合。

3. 使用 <a href="http://weblog.rubyonrails.org/2008/12/17/introducing-rails-metal">Rails Metal</a> 或拆散架構 
Rails 不是萬能框架，也沒必要用 Rails 這種肥重框架單做簡單的事（如 API 的提供）。能改用其他語言或其他框架（<a href="http://www.sinatrarb.com/ ">Sinatra</a>）都是可以嘗試的方法 ...。
Sinatra with ActiveRecord 的整合方法，我寫了一份 <a href="http://github.com/xdite/twitter-message-wall/tree/master">demo</a> 放在 github 上。

4. 檢視 SQL Query 與 Code 效率。
可以使用 <a href="http://www.newrelic.com/">NewRelic</a> RPM ，監視整個網站。找出下的不好的 query，或者是寫的不好的 ORM 語法改善。

關於 ActiveRecord 的基礎與進階可以參考 RailsEnvy 出的 <a href="http://envycasts.com/products/advanced-activerecord">Advanced ActiveRecord</a> 和 Pragprog 出的 <a href="http://www.pragprog.com/screencasts/v-rbar/everyday-active-record">Everyday ActiveRecord</a>。少用 join，多用幾個 select 達到同樣的效果，必要時自己手寫 query...

Code 效率可參考 "Deep in Ruby / Rails" 的書單 ...

5. <a href="http://railslab.newrelic.com/scaling-rails">Scaling Rails</a> 
看完照著 tune，能 cache 的就 cache ..

6. Monitor 
<a href="http://hoptoadapp.com/welcome">Hoptoad</a> #500 自動寄信 / 整理 log 
<a href="http://god.rubyforge.org/">God</a>  # 監視誰死掉了，記憶體吃太多，自動重開 ....

7. Deploy 
機器很多架構複雜，update code 重開麻煩。就寫個 <a href="http://www.capify.org/">Capistrano</a> Recipes 幫你的忙吧 ...。也可搭配 <a href="http://www.orug.org/articles/2009/05/18/cooking-with-chef">Chef</a>。

8. <a href="http://ec2onrails.rubyforge.org/">EC2onrails</a>
懶人架構，前提是要<a href="http://blog.xdite.net/?p=734">熟 EC2</a> 與 Capistrano ...

9. 雲端：<a href="http://heroku.com">Heroku</a> / GAE
有興趣可以參考上個月我寫的這一篇的 <a href="http://blog.xdite.net/?p=1152">Sinatra on the Cloud</a>。</blockquote>

其他 Scale 和架構 design 的東西就都屬於 General Scaling Knowledge，就不在這篇專講 Rails 的文章詳述下去...。

* 總結

練到最後其實就是固定訂一些重要的 feed 來看，有新東西就練和寫心得 ...這樣會進步的非常快。只看書當然不可能比直接看 API 和 blog post 學的快...
介紹幾個消息來源是值得訂的：


<blockquote>
<a href="http://www.railsenvy.com/">RailsEnvy</a>
<a href="http://weblog.rubyonrails.org/">Ruby on Rails Official Blog</a>
<a href="http://www.rubyinside.com/">RubyInside</a>
<a href="http://drnicwilliams.com/">Dr. Nic</a>
<a href="http://yehudakatz.com/">Yahuda Katz</a>
<a href="http://techno-weenie.net/">Rick Olson</a></blockquote>

每一次當 Rails 界舉辦 Conf ，例如 <a href="http://www.railsconf.com">RailsConf 2009 </a>或者是 <a href="http://www.actsasconference.com/">Acts as Conference 2009</a>，其實也都還蠻多 slides 和 video 可以看，看完以後知識會長不少 ....

<blockquote>
railsconf 2009 的 <a href="http://railsconf.blip.tv/">video</a> 與 <a href="http://en.oreilly.com/rails2009/public/schedule/proceedings">slides </a>
act_as_conf 2009 的 <a href="http://www.techscreencast.com/tag/actsasconference-2009">video</a> </blockquote>

希望提供這些資訊能讓大家對學習和練習有個方向....
