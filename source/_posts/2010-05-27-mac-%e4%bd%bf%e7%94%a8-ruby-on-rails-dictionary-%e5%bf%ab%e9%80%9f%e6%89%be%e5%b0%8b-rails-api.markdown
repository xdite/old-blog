--- 
wordpress_id: 1777
layout: post
title: "[Mac] \xE4\xBD\xBF\xE7\x94\xA8 Ruby on Rails Dictionary \xE5\xBF\xAB\xE9\x80\x9F\xE6\x89\xBE\xE5\xB0\x8B Rails API"
date: 2010-05-27 23:52:23 +08:00
wordpress_url: http://blog.xdite.net/?p=1777
---
這是最近學到的一個技巧。Mac 上本身有一套 default 字典軟體，不少人貢獻了辭庫擴充它。有人太閒，也<a href="http://priithaamer.com/blog/rails-23-dictionary">把 Ruby on Rails 的 API 也做成了一本辭庫</a>。

也就是說，只要裝了這套辭庫，當我們在程式碼裡面看到不懂的 API，按下右鍵然後 Look up in Dictionary ，就可以很快的把 API 用法叫出來看。不需要去 <a href="http://apidock.com/rails">APIdock</a> 上翻老半天。

<a href="http://www.flickr.com/photos/xdite/4645194056/" title="Ruby on Rails Dictionary by xdite, on Flickr"><img src="http://farm5.static.flickr.com/4065/4645194056_44563d0755.jpg" width="500" height="312" alt="Ruby on Rails Dictionary" /></a>

安裝方法如下 ( in OSX 10.6.x )

1. 下載 <a href="http://f.priithaamer.com/dictionary/Ruby%20on%20Rails%202.3.dictionary.zip">http://f.priithaamer.com/dictionary/Ruby%20on%20Rails%202.3.dictionary.zip</a>
2. Copy 到 ~/Library/Dictionaries，unzip it !
3. 字典 => 偏好設定 => 打勾 Ruby on Rails 2.3 的選項
4. 到 console 端輸入 defaults write com.apple.Dictionary ProhibitNewWindowForRequest -bool TRUE
5. 之後在任何地方就可以把字反白起來查 API...

<a href="http://www.flickr.com/photos/xdite/4645193984/" title="Ruby on Rails Dictionary by xdite, on Flickr"><img src="http://farm5.static.flickr.com/4001/4645193984_8df6e6249f.jpg" width="443" height="246" alt="Ruby on Rails Dictionary" /></a>

