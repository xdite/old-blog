--- 
wordpress_id: 3088
layout: post
title: "\xE4\xBD\xBF\xE7\x94\xA8 guard-livereload \xE5\x8F\x96\xE4\xBB\xA3 Livereload"
date: 2011-09-15 20:46:54 +08:00
wordpress_url: http://blog.xdite.net/?p=3088
---
<p>一年之前，我曾經寫過一篇文章 <a href="http://blog.xdite.net/?p=1791">LiveReload：你的套版好幫手！</a>介紹 <a href="http://github.com/mockko/livereload" title="">LiveReload</a> 這個工具。</p>

<h2>原理</h2>

<p>LiveReload 是個在套版上非常好用的工具。它的原理是偵測該目錄夾，一旦有靜態 asset-file 更動，可透過搭配的 Browser extension 發送通知，Browser 可以即時看到工作成果。</p>

<p>大幅省去 Developer 手動 reload 的麻煩。</p>

<h2>為什麼要換掉 LiveReload?</h2>

<p>LiveReload 有一個麻煩之處，depend on <a href="http://sourceforge.net/projects/rubycocoa/" title="">RubyCocoa</a>。所以如果你是用自己偏好的 ruby 版本的話，使用 LiveReload 要自己再把 RubyCocoa 編上去。</p>

<p>麻煩就在於，<strong>當你升上 Lion 時，RubyCocoa 在 Xcode 上是編不上去的</strong></p>

<h2>Alternative : guard-livereload</h2>

<p>已經習慣了 LiveReload，套版沒有它實在令人痛苦。不過後來在玩別套網頁開發工具 : <a href="http://middlemanapp.com/" title="">Middleman</a> 時。卻發現 Middleman 竟然內建支援「LiveReload」這個功能。而神祕的是它卻與 RubyCocoa 版的那套似乎完全無涉？</p>

<p>後來 @evenwu 告訴我說，middleman 的這個功能是用另一套 alternative : guard-livereload 組起來的。</p>

<p>而且這套 altenative 是用不同的機制實作出來的。</p>

<h2>直接在 Pow 上使用</h2>

<p>middleman 是一套基於 sinatra / padrino 的網頁開發工具，支援了很多暴力 feature： compass / coffeescript / sprockets / livereload。@evenwu 也有寫過<a href="http://rgba.tw/post/10074630322/middleman-intro">專文</a>介紹它。</p>

<p>middlman + livereload 的用法是 : </p>

<p><pre></p>
<p>$ middleman server --livereload</p>
<p></pre></p>

<p>但我真正想要的只有 livereload，我不想要每次想拿這個功能都得開 middle man server --livereload。（middleman 是透過 middleware 掛起 guard-livereload 的）。於是我就開始研究如何只取用 livereload，這樣才能直接搭配 Pow。</p>

<p>後來發現其實也頗簡單，文件就已經寫在 <a href="https://github.com/guard/guard-livereload" title="">guard-livereload</a> 上。</p>

<p>i.e. <a href="https://github.com/guard/guard" title="">guard</a> 是一套監視目錄下檔案變動的工具，監視變動然後 do_something。</p>

<p>在 project 下的 Gemfile 下掛上 guard-livereload。</p>

<p><pre></p>
<p>gem 'guard-livereload'</p>
<p></pre></p>

<p>然後在 project 下輸入以下指令</p>
<p><pre> </p>
<p>guard init livereload</p>
<p></pre></p>

<p>會產生一個 Guardfile，根據你的需求修改之。</p>

<p>接著，執行</p>

<p><pre></p>
<p>guard</p>
<p></pre></p>

<p>這樣就可以把 guard-livereload 跑起來了。等同於 LiveReload 原生效果，也 compatiable 原先 <a href="https://chrome.google.com/webstore/detail/jnihajbhpnppcggbcgedagnkighmdlei" title="">LiveReload 的 Chrome plugin</a></p>

<p>這樣就可以直接跟 Pow + Chrome Extension 接起來了。</p>

<p>各位歡迎試試看吧。</p>

<div class="note">

廣告：
<ul>
	<li> 本週六 (09/17) 下午一點半，我在 HTML5 / Nodejs 聯合小聚有場 <a href="http://www.facebook.com/event.php?eid=108571625914197">HTML5 語意實戰 talk</a>，歡迎捧場。</li>
   <li> 想自學 Rails 嗎？ 我編了一本書 <a href="http://rails-101.heroku.com/books/1-rails-101"> Rails 101 </a> for 新手自學。只要 $ 9.99 USD。 </li>
</ul>
</div>
