--- 
wordpress_id: 1557
layout: post
title: "Rack \xE8\x88\x87 Rack middleware"
date: 2010-02-21 18:32:26 +08:00
wordpress_url: http://blog.xdite.net/?p=1557
---
Rails 2.3 引進了 <a href="http://rack.rubyforge.org/">Rack</a>，不少 ActionController 內部裡的 component 都改成用 Rack middwares 實做了。一般在 rails project 內，鍵入

<blockquote> rake middle
</blockquote>

可以看到這個 project 用了哪些 rake middleware 。

<blockquote>
use Rack::Lock
use ActionController::Failsafe
use ActionController::Session::CookieStore, , {:secret=>"<secret>", :session_key=>"_<app>_session"} 
use Rails::Rack::Metal 
use ActionController::RewindableInput 
use ActionController::ParamsParser 
use Rack::MethodOverride 
use Rack::Head 
use ActiveRecord::QueryCache 
run ActionController::Dispatcher.new 
</blockquote>

在 Rails3 中，更是針對這部份做了大大的強化：新的 route 部份任何一個 endpoint 都可以輕易接上支援 Rack 的 App、ActionController::Base 也繼承自 Metal （出處 : <a href="http://www.engineyard.com/blog/2009/my-five-favorite-things-about-rails-3/">My Five Favorite Things About Rails 3</a> #2 ）。

但是，到底什麼是 Rack、什麼是 Rack middleware，Rails 如此大幅的支援 Rack，會帶來什麼樣重大的變革，恐怕很多開發者還是一頭霧水。而這就是我寫這篇文章的目的。

<strong><big>什麼是 Rack？</big></strong>

如果你在網路上，搜尋 Rack 相關的資料的話，或許會找到這一篇文章：<a href="http://article.yeeyan.org/view/blackanger/18184?from_com"> Ruby on Rack #1 - 與Rack的第一次親密接觸</a>

裡面寫到 Rack 是：

<blockquote>Rack為使用Ruby開發web應用提供了一個最小的模塊化和可修改的接口。用可能最簡單的方式來包裝HTTP請求和響應，它為web 服務器，web框架和中間件的API進行了統一併提純到了單一的方法調用。</blockquote>

看不懂 ...換成<a href="http://m.onkey.org/2008/11/17/ruby-on-rack-1">原文</a>好了 ...

<blockquote>Rack provides a minimal, modular and adaptable interface for developing web applications in Ruby. By wrapping HTTP requests and responses in the simplest way possible, it unifies and distills the API for web servers, web frameworks, and software in between (the so-called middleware) into a single method call.</blockquote>

有點清楚了，但是實在還是不懂 Rack 的意義在哪裡？

<strong>"Rack provides a minimal, modular and adaptable interface for developing web applications in Ruby"</strong>

一般的印象，不是 Framework 跟 Web Server 對接就好，為什麼需要 Rack 這個東西卡在中間呢？

現實的世界當然沒有這麼直觀，隨著 Ruby Community 蓬勃的發展， 為了不同的開發目的，出現了很多 Framework：Rails , Sinatra , Merb ...，而大家為了自己的 production 需求，又出現了相當多不同的 web server : Mongrel / Thin / Passenger / Unicorn ...。

<strong>多對多的混亂</strong>就會出現了，我是 web server 的開發者，需不需世界上每誕生一套 Framework 就支援它呢？每一套 Framework 還有版本上的問題 ...

這時候 

<strong>"Rack provides a minimal, modular and adaptable interface for developing web applications in Ruby"</strong>

就顯得相當重要了

<a href="http://www.flickr.com/photos/xdite/4373500414/" title="Flickr 上 xdite 的 Rack / Server / Framework"><img src="http://farm3.static.flickr.com/2793/4373500414_54b6769161.jpg" width="500" height="375" alt="Rack / Server / Framework" /></a> ( 圖片出處：<a href="http://en.oreilly.com/railswinter10/public/schedule/detail/13304">Rack in Rails3</a> ）

Rack 的出現，使得混亂的情形不再。它提供了一套 <a href="http://rack.rubyforge.org/doc/files/SPEC.html">Rack SPEC</a> 給 Server 和 Framework 遵循，解決了這個問題。

Rack 的 SPEC 非常簡單，就是一個 call method，吃一個 enviroment 參數，然後回傳一個 array，裡面包含 status , header, body。

<strong><big>什麼是 Rack middleware？</big></strong>

那有了 Rack，為什麼不能在 Rack 和 Application 中間幹點壞事呢？ Rack middleware 說穿了，其實就是一些 follow Rack SPEC 的 application。Rack middleware 是可以堆疊的，基本上它的概念有點像 before / after / around 這種 filter。像 Rails 2.3 以後，其實每個 request 進來是要經過很多層 middleware 才打進 application 的，每一層都攔下來作某些事再傳到下一層去。

<a href="http://www.flickr.com/photos/xdite/4373651644/" title="Flickr 上 xdite 的 rack stack"><img src="http://farm5.static.flickr.com/4019/4373651644_a4f40bce21.jpg" width="500" height="156" alt="rack stack" /></a>
寫 Rack middleware 和用 Rack middleware 有什麼好處呢？

Well, 很多邏輯簡單但 Request 數量又很大的行為（比如 Ajax Call , API ）就可以寫 Rack middleware ，一旦符合 Pattern 就攔截下來處理掉，直接傳回去。而不需要真正打進 Framework 裡面進去處理，畢竟用 Fraemwork 去處理小事是殺雞焉用牛刀...

<a href="http://wiki.github.com/rack/rack/list-of-middleware ">List of Middleware</a>、<a href=" http://github.com/rack/rack-contrib">rack-contrib</a>、<a href="http://coderack.org/">Coderack</a>，這些地方提供了一大堆非常有意思的 Rack middleware !!

<strong><big>為什麼 Rails 要大幅支援  Rack ？</big></strong>

理由很顯見的：

1. 支援標準，可以輕易的跟各種 Web Server 接在一起。
2. 透過 Rack middleware 和 Rails metal 可以輕易的在內部，跟其他 Framework 整合在一起。

像上一段提到的 邏輯簡單但 Request 數量又很大的行為，除了可以用 Rack middleware 處理外，還可以透過 <a href="http://soylentfoo.jnewland.com/articles/2008/12/16/rails-metal-a-micro-framework-with-the-power-of-rails-m">Rails Metal </a> 的機制 bypass 到其他比較輕量的 framework 去處理。

在 Rails 2.3 之中，Rails Metal 算是獨立出來的一塊，進來的 request 會 "跳過" ActionController stack，直接給 Rails Metal Application 處理。

但在 Rails 3 中，Metal 成為了 ActionController 的一部份。

<a href="http://www.flickr.com/photos/xdite/4378512734/" title="Flickr 上 xdite 的 metal inher"><img src="http://farm5.static.flickr.com/4013/4378512734_785b8b5e0e_o.png" width="469" height="145" alt="metal inher" /></a>

<strong>Rails3 更進一步的將 "controller 每一個 action" 都變成了 Rack app</strong>，除了在 route 中每一個 endpoint 可以輕易的接上 Rack app 之外，甚至你可以使用 controller 中的 redirect 直接接上 Rack App. 以下是原理: 

<a href="http://www.flickr.com/photos/xdite/4374797389/" title="Flickr 上 xdite 的 redirect rack"><img src="http://farm5.static.flickr.com/4033/4374797389_d9e3291f52.jpg" width="500" height="174" alt="redirect rack" /></a> ( 圖片出處：<a href="http://sdruby.org/podcast/69">Rails 3: From Vaporware to Awesomeness in 12 Months</a> ）

將"在 Rails 整合其他 Framework "的這件事的方便程度提升至一個極致。好處當然就是能讓 code 變得更乾淨，節省更多效能，以及在 Rails 做出以往無法做出的應用（搭配其他 Framework 在內部使用）。

比如說最近這篇文章 <a href="http://www.web2media.net/laktek/2010/02/16/building-real-time-web-apps-with-rails3/">Building Real-time web apps with Rails3</a> 就是一個很好的例子 ( Rails + Cramp ）。

支援 Rack 不重要嗎？很重要 :D 

That is "Why Rack?".

