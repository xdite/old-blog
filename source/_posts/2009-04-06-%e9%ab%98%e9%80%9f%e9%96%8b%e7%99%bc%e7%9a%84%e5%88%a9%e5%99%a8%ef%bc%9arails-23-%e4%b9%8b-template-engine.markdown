--- 
wordpress_id: 1133
layout: post
title: !binary |
  6auY6YCf6ZaL55m855qE5Yip5Zmo77yaUmFpbHMgMi4zIOS5iyBUZW1wbGF0
  ZSAmIEVuZ2luZQ==

date: 2009-04-06 01:54:30 +08:00
wordpress_url: http://blog.xdite.net/?p=1133
---
上個月底，Rails 發布了最新的版本：2.3。這個版本更新的數量也是歷史之最。光看 <a href="http://guides.rubyonrails.org/2_3_release_notes.html">Release Note</a> 就直叫人興奮。 

當中我覺得最有爆炸性的就是 Template & Engine。

如果說促成 Rails 高速開發的關鍵，是一幫瘋子沒日沒夜一直生出的 plugin。那麼 Template + Engine 的出現，將會讀高速開發的神話推向另外一個高點。

怎麼說呢？

開發者不可能只作一個專案，當你一個專案一個專案一直寫下去，必然會發現許多 code 是重複的。plugin 很好用，但寫久了發現基本的 plugin 用來用去也是那幾個。開新專案老是要一個一個 script / plugin 也很煩。最後的解法必然是開始維護自己的架站包。但架站包又會有需要更新 plugin，merge / remove 不需要的 code / plugin 的問題。

Rails 2.3 中的 <a href="http://m.onkey.org/2008/12/4/rails-templates">Template</a> 大致上的用意就是

<blockquote>讓你編寫自己的架站包 config。產生新專案時可以讓 Rails 依著這個 template 自動安裝自定義的 plugin , gem , route, rake 甚至 generate model / controller  等等 ...</blockquote>
 
而 Rails 2.3 的 <a href="http://rails-engines.org/">Engine</a> 就更威了。（我直接取用<a href="http://blog.xdite.net/?p=1126">上一篇文章</a> Rails : Engine 的介紹）

<blockquote>以往在寫 Rails Application 時，想要撰寫/分享一些特殊的功能。往往只能透過 blog 教學或者架設一個 demo application，讓人 copy & paste 程式碼回去自行 merge 回去 controller 或 model 裡。 Rails 2.3 Enigne 這項新 feature 可以讓你把別人寫的「整個網站」/「整個功能」掛起來當作一個 plugin (Rails applications that can be embedded within other applications)。</blockquote>

這兩者合起來能夠造成的效果，就是開發者能更快更方便的使用自己以前寫過或他人的 project 整合進自己的 Application，而且能夠無縫（這裡有些誇飾，應該是說縫變得極小）、全自動化。

舉例來說，今天如果我想要開發一個吃 twitter / openid / yahoo 認證的 blog。在以往的版本，也許我要打開我以前寫過的 project，花上幾個小時剪剪貼貼 controller code，寫個一兩百行 code 來整合加上 debug & test。Rails 2.3 的 Template + Engine 出現之後，比較誇張的說法是，也許我需要做的只是 plugin 掛掛掛，多餘的程式碼砍砍砍 ...這樣就可以上線了...。

令人十分興奮的 Feature 啊...。

延伸閱讀：<a href="http://drnicwilliams.com/2009/03/30/closing-in-on-the-dream-one-click-to-deploy-rails-apps/">Closing in on The Dream: “one-click-to-deploy Rails apps” </a>
