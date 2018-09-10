--- 
wordpress_id: 1795
layout: post
title: !binary |
  5ZyoIE1hYyDkuIrlu7rnq4vkub7mt6jnmoQgUmFpbHMg6ZaL55m855Kw5aKD
  ICggT1NYIDEwLjYgU25vdyBMZW9wYXJkICkg

date: 2010-08-13 11:43:08 +08:00
wordpress_url: http://blog.xdite.net/?p=1795
---
自從 10.6 大改底層之後，在 Mac 上面安裝乾淨的 Rails 開發環境，一向是個痛。在裝過快十遍的 Rails Developing Environment 後，我整理出了一個接近 best practice 的版本，現在放出來供大家參考。


<blockquote>
* Install Xcode ( 出廠的原裝 CD 有，裝完請跑 Software Update ）
* Install "git-osx-installer": <a href="http://code.google.com/p/git-osx-installer/">http://code.google.com/p/git-osx-installer/</a>
* Install "homebrew"：<a href="http://github.com/mxcl/homebrew">http://github.com/mxcl/homebrew</a>
* brew install mysql
* 依照指示設定你的 mysql ，然後把 mysql 跑起來
* brew install imagemagick
* brew install ruby-enterprise-edition

<script src="http://gist.github.com/522233.js"> </script>


* passenger-install-apache2-module
* 去系統設定 panel 上面打開網頁分享。編輯 /private/etc/apache2/httpd.conf 加上 mod_rails
* 設定  /private/etc/apache2/extra/httpd-vhosts-conf : <a href="http://gist.github.com/520602">http://gist.github.com/520602</a>
* 都改好後 sudo apachectl restart
* 這是內建預設的 webroot directory  : /Library/WebServer/Documents/</blockquote>


===

- 常見 Q & A

Q: Why not using Macports or fink ? 
A: 更新速度慢, 會沒事裝一些你不需要的東西, 系統最後相當不乾淨. 所以會改用 brew, 有關於 Mac 套件管理方面的 issue 可見 <a href="http://www.engineyard.com/blog/2010/homebrew-os-xs-missing-package-manager/">Engineyard 之前寫的 Homebrew 介紹文</a>。

Q: 一定要照順序安裝嗎？
A: 是的。否則你會撞到最痛的 MySQL 與 mysql gem、ImageMagick 與 Rmagick gem 問題。非常非常非常的痛....如果你是 Mac + Rails 新手，別多問，照裝就是了。

Q: 為什麼要使用 <a href="http://www.rubyenterpriseedition.com/">ruby enterprise edition</a> 以及 <a href="http://www.modrails.com/">mod_rails</a> ?
A: best practice 啊，孩子。

Q: 為什麼你會沒事裝十遍開發環境 ?
A: 因為我的 team 裡面的 mac 都是我在裝啊....(淚)。不幫他們裝，事後解除地雷的成本比裝機器成本高的太多了。不然你以為我很喜歡裝機器嗎?


