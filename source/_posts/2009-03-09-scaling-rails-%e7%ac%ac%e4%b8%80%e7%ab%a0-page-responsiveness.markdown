--- 
wordpress_id: 995
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xB8\x80\xE7\xAB\xA0 Page Responsiveness"
date: 2009-03-09 09:00:45 +08:00
wordpress_url: http://blog.xdite.net/?p=995
---
在開始講述 Scaling 技巧之前，我們先要知道什麼是 Responsiveness。

Responsiveness 就是 The amount of time it takes to load a web on a browser ( 開這一張網頁要花上多久時間 loading ）。因此在這一集之中，會涵蓋兩個主題：

<ol>
	<li>How do you measure responsiveness ? (怎麼測量）</li>
	<li>How do you improve responsiveness ? (怎麼改善）</li>
</ol>

<h2>How do you measure responsiveness ? </h2>

有兩種方式：

1. 安裝 Firefox 的 Plugin : Firebug，使用「Net」功能，可以對頁面各項元素作 Loading 測量。

<a href="http://www.flickr.com/photos/xdite/3335303837/" title="Flickr 上 xdite 的 vlcsnap-16498659"><img src="http://farm4.static.flickr.com/3644/3335303837_d7c2e4e771.jpg" width="500" height="375" alt="vlcsnap-16498659" /></a>

2. 打開 Safari 中 Advance 選項中的 "Show Developer Menu"，然後使用 Deverloper Menu 中的 Web Inspector 中的 「Netowrk」，一樣可以得到前者的效果。

<a href="http://www.flickr.com/photos/xdite/3335304043/" title="Flickr 上 xdite 的 vlcsnap-16499586"><img src="http://farm4.static.flickr.com/3651/3335304043_975035cc38.jpg" width="500" height="375" alt="vlcsnap-16499586" /></a>

使用這兩個工具其一，去觀看 loading Railsenvy.com 的數據，會發現絕大多數的時間都耗在 loading 頁面上，而非頁面產生時間，因此針對 Server Side 作效能改進是比較無意義的。

<a href="http://www.flickr.com/photos/xdite/3335304135/" title="Flickr 上 xdite 的 vlcsnap-16500302"><img src="http://farm4.static.flickr.com/3416/3335304135_41aa0283aa.jpg" width="500" height="375" alt="vlcsnap-16500302" /></a>

應該想辦法減少的是降低 loading 頁面所需的時間。

<a href="http://www.flickr.com/photos/xdite/3335304217/" title="Flickr 上 xdite 的 vlcsnap-16500633"><img src="http://farm4.static.flickr.com/3661/3335304217_a8ec9c84c2.jpg" width="500" height="375" alt="vlcsnap-16500633" /></a>

<h3>How do you improve responsiveness ? </h3>

回到 improve responsiveness。有兩件事是我們可以做的：

<ol>
	<li>By Improving Server Performance </li>
	<li>By Imporving Browser Load Time</li>
</ol>

這時候你需要安裝另外一大利器：Firefox 的 Plugin : YSlow。YSlow 在測量完你的頁面之後會給你一些實際的建議。

<a href="http://www.flickr.com/photos/xdite/3335304441/" title="Flickr 上 xdite 的 vlcsnap-16501318"><img src="http://farm4.static.flickr.com/3594/3335304441_54d701f5bb.jpg" width="500" height="375" alt="vlcsnap-16501318" /></a>

<h3>第一項：Make Fewer HTTP Request</h3>

Rails 提供了 js / css 的 cache 選項。

[Ruby]
<%= javascript_include_tag , :all, :cache => true %>
[/Ruby]

會在 production 模式中把你頁面所用到的 javascript ，統統 combind 成一個 all.js。

[Ruby]
<%= javascript_include_tag , :main , :cache => true %>
[/Ruby]

這個選項是可以 customize 的，比如你想把結果包成 main.js。

[Ruby]
<%= javascript_include_tag , "product", "cart" ,"chckecout" , :cache => "shop" %>
[/Ruby]

把其中三個特定的 js ( product.js, cart.js, checkout )，包成一個 js ( shop.js)。

[Ruby]
<%= stylesheet_link_tag :all , :cache => true %>
[/Ruby]

同理，stylesheet 也可如法炮製。

如果你的網站需要 load 4 支 javascript，4 支 css。這時候可用此法，壓成 1 支 javascript，1 支 CSS。原先需要 8 個 request，現在只需要 2 個，足足少了 6 個！

順帶一提 : 

1. 影片推薦大家安裝 <a href="http://github.com/sbecker/asset_packager/tree/master">asset_packager</a> plugin，它可以同時 combind 和 minify 你的 javascript 和 CSS。（ minify 就是幹掉空行，壓縮檔案大小）

2. 請愛用 <a href="http://code.google.com/apis/ajaxlibs/ ">Google AJAX Libraries</a>，當要 load 比較常見的 libraries 的時候都用 Google host 的。因為除了從 Google load 可能比較快之外，如果訪客拜訪了其他人的網站，其他人的網站也使用了 Google host 的版本，這樣訪客訪問你的網站時，就不需要再 load 一次。

<h3>第二項：Use a CDN</h3>

CDN 是 Content Delivary Netowork 的縮寫。使用 CDN 放 靜態檔案的好處是：假設你的訪客來自全球各地，CDN 吐檔案會使用離你的訪客速度最優的機器吐檔案。

<a href="http://www.flickr.com/photos/xdite/3336250086/" title="Flickr 上 xdite 的 vlcsnap-16533310"><img src="http://farm4.static.flickr.com/3652/3336250086_807e0d5b1e.jpg" width="500" height="375" alt="vlcsnap-16533310" /></a>

你可能之前就聽過幾個比較大的 CDN 如：<a href="http://www.akamai.com">Akamai</a>、<a href="http://www.cdnetworks.com">CDNetoworks</a>、<a href="http://www.limelightnetworks.com/">Limelight Networks</a>。但如果你剛想學用 CDN，影片推薦大家可以從 <a href="aws.amazon.com/cloudfront/ ">Amazon Cloudfront</a> 開始練習（因為也比較便宜...）。

Rails 也提供了方便的方式讓你將靜態檔案上 CDN。

將此行寫在 config 檔裡：

[Ruby]
ActionController::Base.asset_host = "assets.example.com"
[/Ruby]

當使用

[Ruby]
 <%= image_tag("rails.gif") %>
[/Ruby]
 
會自動生成

[Ruby]
 <img src ="http://assets.example.com/images/rails.gif" >
[/Ruby]

<h3>第三項 : Add an Expires header</h3>

簡單來說，是這樣運作的：為靜態檔案加上 expire header，告訴訪客的瀏覽器此檔案在某月某日某時前，都是 valid 的。比如說，該訪客造訪網站於 3 月 1 日，已經要過了一次靜態檔案，此靜態檔案 expire 日是 3/30。這樣下次訪客於 3/30 前不管造訪幾次，都會使用 Client 中已經 cache 的靜態檔案，而不會每次來都每次跟 Server 要。

<a href="http://www.flickr.com/photos/xdite/3336282556/" title="Flickr 上 xdite 的 vlcsnap-16541655"><img src="http://farm4.static.flickr.com/3608/3336282556_ef5b5db1fe.jpg" width="500" height="375" alt="vlcsnap-16541655" /></a>

影片當中提供了 Nginx 與 Apache 中的實做方法。

<a href="http://www.flickr.com/photos/xdite/3336290672/" title="Flickr 上 xdite 的 vlcsnap-16545558"><img src="http://farm4.static.flickr.com/3367/3336290672_d26d9c246a.jpg" width="500" height="375" alt="vlcsnap-16545558" /></a>

但這樣作有一個危險性，如果你修改了你的 CSS / JS，並且 Deploy 上 Server 了。但當初設的 expire 時間還沒到，這樣 client 只會繼續使用舊版的檔案，而不會重要。那要如何解決這個問題？

Rails 面對這樣的問題有 default 設計，解決了這個問題：

當你使用這類的 helper 時：

<a href="http://www.flickr.com/photos/xdite/3335474955/" title="Flickr 上 xdite 的 vlcsnap-16549246"><img src="http://farm4.static.flickr.com/3659/3335474955_e1ae679855.jpg" width="500" height="375" alt="vlcsnap-16549246" /></a>

產生出來的 URL 會是這樣，後面多加了 modified time。

<a href="http://www.flickr.com/photos/xdite/3336308834/" title="Flickr 上 xdite 的 vlcsnap-16547355"><img src="http://farm4.static.flickr.com/3629/3336308834_272e6d05a5.jpg" width="500" height="375" alt="vlcsnap-16547355" /></a>

如果你 update 了 JS/CSS，後面的數字就會改變。這樣一來，靜態檔案即使改變了，但卻不會受到 expire header 的影響。因為加了 modeified time 後，改變前與改變後所產生的靜態檔案 URL 是不相同的，因此 client 就會去抓最新的檔案，而避免掉前述的問題。

YSlow 的其餘改進方法，作者限於篇幅沒有講述，但他請你有空的話可以去看 <a href="http://developer.yahoo.com/yslow/help/">YSlow 的 document</a>，上面有方法可以教你如何改善其他的情形。

本篇 Video 的內容比較偏減少 Respond Time，之後的 Video 將會側重 Improve Server Performance。 
