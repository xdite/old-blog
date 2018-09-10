--- 
wordpress_id: 1597
layout: post
title: "Scaling Rails Site\xEF\xBC\x9AReading Material # 1"
date: 2010-02-28 01:10:23 +08:00
wordpress_url: http://blog.xdite.net/?p=1597
---
昨天 <a href="http://ihower.tw">@ihower</a> FW 了一封信給我。 內容大概是有 Ruby Community 的朋友寫信給他，想要瞭解 Rails Production site 的架構設計與 Scaling 觀念與技術，想請 ihower 給他一點方向。於是 ihower 又把這封信 FW 給我，我們稍微聊了幾分鐘。所以就有了這一系列的文章。

目前市面上並沒有一本專書是專門教 Scaling Rails Application 的。但是有關於這個議題，是真的有不少東西可以讀的。

大致上可規類幾個方向：

<blockquote>
1. Rails performance
2. Front-end web performance
3. Caching & HTTP Reverse Proxy Caching
4. Asynchrony Processing (Message queue)
5. Partition Component using SOA 
6. Distributed Filesystem / Database , Database Optimization, NoSQL
</blockquote>

1. <big><strong>Rails (Application) performance </strong></big>

Rails 這個框架本身的 Performance 並不在這個議題討論之內。原因是 Rails 2 本身的 req/s 已經在 framework 界的表現算是不錯，還電掉一堆 PHP framework，而且對於 Rails 本體的表現，很多時候 Application 純開發者是沒有什麼作為能力的。Tuning 的原則建立在於認定 Rails 能夠有一定的效率表現，因此方向改於著重於避免設計出爛架構、寫出爛 code 造成效率低落，以較為良好的架構設計達到避免將壓力加諸於 Rails 身上的目標。

* <strong>換掉 Ruby 版本</strong>

如果要提升 Rails framework 的速度，換掉 Ruby VM 是最快的。但如果還是希望使用純 C 寫的 Ruby ( gem compatible 問題），可考慮使用 <a href="http://www.rubyenterpriseedition.com/">Ruby Enterprise Edition (REE)</a>。光換成 REE，記憶體用量馬上就少上 1/3，而速度也快上 1/3。

關於 REE 能夠做到這件事的原理，已經寫在 <a href="http://www.rubyenterpriseedition.com/faq.html">FAQ</a> 裡，而他們在 2009 年底也在<a href="http://www.rubyenterpriseedition.com/faq.html"> Google Tech Talk 上給了一場相關的 talk</a>。

[關鍵字: Ruby VM ]

<strong>* 抓出 slow action / slow ruby code，並針對這些部分改善</strong>

[ 可安裝 <a href="http://www.newrelic.com/">NewRelic RPM</a> 抓出內部的問題。 ]

很多時候，Rails Application 的緩慢，並不是 Rails 這個 framework 產生的問題。而是開發者 <strong>*完全* 不熟 Rails API 或者是 Ruby 這個語言</strong> 本身，或者是濫用 Rails 的開發方便度寫出爛架構所造成的問題。常見的情形有以下幾種狀況：

- <strong>不熟 Ruby</strong>。可以用 Ruby 本身提供的 method 一行就解決的事情，自己寫了迴圈處理，造成了 object 大量的被 clone 出來浪費記憶體。

<blockquote>
解法：閱讀 <a href="http://ihower.tw/blog/archives/1691">Writing Efficient Ruby Code</a>，以及閱讀 <a href="http://www.manning.com/black2/">The Well-Grounded Rubyist</a>（ Ruby for Rails 的新版）、<a href="http://www.informit.com/store/product.aspx?isbn=0321604180">Refactoring: Ruby Edition</a> </blockquote>

- <strong>不熟 Rails API</strong>。比如說：不熟 ActiveRecord，導致一個簡單 action，就產生了極大量的 query。或者是不懂做 counter cache，造成了 database 極大的壓力；大量使用 render :partial，卻不知道 render :partial 是極其昂貴的，應該把常用的 partial cache 住。

<blockquote>解法：閱讀 <a href="http://peepcode.com/products/advanced-activerecord">Advanced ActiveRecord</a> 、<a href="http://en.oreilly.com/rails2008/public/schedule/detail/2032">Advanced Active Record Techniques: Best Practice Refactoring</a>、<a href="http://www.engineyard.com/blog/2009/thats-not-a-memory-leak-its-bloat/">That’s Not a Memory Leak, It’s Bloat</a>、<a href="http://peepcode.com/products/rails-code-review-pdf">Rails Code Review</a>、<a href="http://www.railsrescuebook.com/toc">Railsrescue Handbook</a> </blockquote>

- <strong>plugin 的錯</strong>。plugin 作者並不是全知全能，他們開發 plugin 只是為了順手解決自己手邊遇到的問題。你安裝的 plugin 若是熱門 plugin，效能上應該可能沒什麼問題，因為大多已經在許多人的 production 上千錘百鍊過，有問題也幾乎 patch 掉了。但若是比較冷門或比較新的，可能這個 plugin 就是造成你 performance 低落的元兇。

- <strong>特殊 action 造成的效能低落</strong>。在 Rails Application 上，有一個常見功能是開發者的痛，就是上傳檔案的問題。上傳檔案這個行為，常常會造成 block 住整個站的現象。而 Merb 這個 Framework，當初就是設計來解決 Upload File 的問題的。很痛的還有寄信的這個功能，Rails 的 AR Mailer 真的頗廢 :/ ，它原始的設計初衷是要讓開發者在 Framework 輕鬆寫出寄信功能之用的，不過總會有天兵開發者會寫出在 action 裡一次寄一千封這種會死人的事....

<blockquote>解法：將效率低落的 action，諸如 upload 功能。利用 <a href="http://railscasts.com/episodes/150-rails-metal">Metal</a> 的機制，bypass 到其他 framework 或者是直接用 <a href="http://blog.xdite.net/?p=1557">Rack middleware</a> 做掉。如果你的 Web Server 最前面是 Nginx 的話，有人寫了一個十分有用的 upload module，<a href="http://brainspl.at/articles/2008/07/20/nginx-upload-module">解決了這個問題</a>。至於寄信或者需要耗費大量資源的 action（如擷圖、縮圖、縮影片），應該用 queue + worker 加上 3rd party service 做掉。比如說寄信的方式，我就會推薦用 <a href="http://github.com/tobi/delayed_job">Delayed Job</a> +<a href="http://developer.madmimi.com/"> Madmimi gem</a> 解決。網站截圖就直接丟 <a href="http://webthumb.bluga.net/home">bluega</a> 處理。</blockquote>

- <strong> API 不應該直接使用 Rails 實作</strong>。API 通常多屬於邏輯簡單但 Request 數量又很大的行為，request 打進 Rails 本身就會造成一定量的負擔。這部分的實作部分也建議 bypass 到其他 framework 、直接用 <a href="http://blog.xdite.net/?p=1557">Rack middleware</a> 做掉，或乾脆切出去用其他語言/架構 implement。

至於更多 profiling memory usage 以及 measure code efficiency 的工具，ihower 將在 <a href="http://ihower.tw/blog/archives/3890">Ruby Tuesday #10</a> 講到，這方面就交給他了，它的主題是 Rails Performance。而我本人也會在 Ruby Tuesday #10 講一場 Scaling Rails Site。

<div style="width:425px" id="__ss_3299364"><strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/ihower/rails-performance" title="Rails Performance">Rails Performance</a></strong><object width="425" height="355"><param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=rails-performance-2010226-100228083842-phpapp01&stripped_title=rails-performance" /><param name="allowFullScreen" value="true"/><param name="allowScriptAccess" value="always"/><embed src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=rails-performance-2010226-100228083842-phpapp01&stripped_title=rails-performance" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed></object><div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/ihower">Wen-Tien Chang</a>.</div></div>

我個人推薦這方面的閱讀材料是：

<a href="http://railslab.newrelic.com/2009/10/23/episode-19-on-the-edge-part-1">
Scaling Rails : On The Edge - Part 1</a>
<a href="http://railslab.newrelic.com/2009/10/23/episode-20-on-the-edge-part-2">Scaling Rails : On The Edge - Part 2</a>
<a href="http://railslab.newrelic.com/2009/10/23/episode-21-on-the-edge-part-3">Scaling Rails : On The Edge - Part 3</a>
