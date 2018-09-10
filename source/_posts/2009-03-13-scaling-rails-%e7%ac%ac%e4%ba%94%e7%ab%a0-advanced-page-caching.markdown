--- 
wordpress_id: 1020
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xBA\x94\xE7\xAB\xA0 Advanced Page Caching"
date: 2009-03-13 09:00:50 +08:00
wordpress_url: http://blog.xdite.net/?p=1020
---
當你使用第二章提到的技巧，將 index page 開心的 cache 住了以後。很快就會像李組長一樣，眉頭一皺，發現案情並不單純（這是 PTT 梗）。

如果 index 拉出來的 @objects 使用了 paginate，很快你就會發現，第二章提到的技巧，會讓你的分頁設計失效，因為 cache 結果永遠會 return 第一頁。

那麼要怎樣解決這個問題呢？解法：是為分頁定製特殊的 route：

到 cofing / routes.rb

[Ruby]
 map.posts_with_pages '/posts/page/:page', :controller => 'posts', :action => 'index'
[/Ruby]

這樣就解決掉了問題。原先的 Page Caching 會將 index 頁，cache 成 /posts.html。而新法會將結果 cache 成 /posts/page/1.html、/posts/page/2.html，問題解決！

但這解法，又會讓你眉頭一皺。不只 Pagination 的問題需要解決，只要頁面上有 Dynamic Data，Cache 就很搞？

比如說頁面上的 Login 狀態好了，這要怎麼更新呢？

解法是，用 Ajax Dynamic Loading 解決這個問題：

[Ruby]
<p id ="login_status" style="color: blue"> <%= login_status %></p>
[/Ruby] 

load 進 dafault 的 javascipt library

[Ruby]
<%= javascript_include_tag :all %>
[/Ruby]

在頁面的最下方，寫一段 javascript 作 update 的動作。

[Ruby]
<script type ="text/javascript" charset ="utf-8">
  <%= remote_function :update => 'login_status', :url => login_status_path %>
[/Ruby] 

然後在 SessionsController 裡訂製一個 action

[Ruby]
  def status
     render :inline => login_status
  end
[/Ruby]

這樣 index 結果即使 cache 住了，但是因為 login status 的部份是用 Javascipt 去 Load 出來，所以還是會顯示正確的結果。


不過在影片中，Greg 認為 Login / Logout is Overrated 而且被 Overused 了。他認為沒必要每個頁面都放置這個資訊。它解釋了原因，以 <a href="http://rails.lighthouseapp.com/dashboard">Lighthouse</a> ( 可在此 submit 對於 Rails 的 ticket )為例：

<a href="http://www.flickr.com/photos/xdite/3337270150/" title="Flickr 上 xdite 的 vlcsnap-51236"><img src="http://farm4.static.flickr.com/3315/3337270150_8b0a0b337b.jpg" width="500" height="375" alt="vlcsnap-51236" /></a>

Greg 覺得這個網站特別不需要這種設計。以 Envycast 為例：不管是否登入，bar 永遠一樣。反正點下去，權限不對，還是會把你導到 Login 頁面去。

<a href="http://www.flickr.com/photos/xdite/3336442091/" title="Flickr 上 xdite 的 vlcsnap-52069-crop"><img src="http://farm4.static.flickr.com/3399/3336442091_40dfb6f2bd.jpg" width="363" height="90" alt="vlcsnap-52069-crop" /></a>

那要怎麼登出呢？Click「Account」 就會產生下拉選單。

<a href="http://www.flickr.com/photos/xdite/3336443193/" title="Flickr 上 xdite 的 vlcsnap-52930-crop"><img src="http://farm4.static.flickr.com/3398/3336443193_d8f97cbcf2_m.jpg" width="240" height="237" alt="vlcsnap-52930-crop" /></a>

即使最慘的情形發生好了：假使你偶然間離開了你的電腦，別人在你不知情的情況下打開了這個網站使用，他也不知道你沒登出。需要害怕信用卡資訊洩漏嗎？

EnvyCast 付款的機制是 PayPal，因此不會發生這種情形...

再回到 Lighthouse 這個例子上，最慘的情形是：如果你忘記了登出，別人卻使用了你的店法，對 Rails 做出了貢獻 ...XD ( 以你之名）

Greg 認為有些重要的網站或特定的情形，是需要 Login Status 這種設計的。比如說銀行帳戶頁、Amazon 帳戶、或者此網站的 Profile 儲存了你的信用卡號碼。

但大多數的網站是不需要這種設計的，特別是這網站上根本沒有儲存任何重要或高度敏感資料設計的時候。

下一集要談的是 Action Caching。
