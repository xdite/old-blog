--- 
wordpress_id: 3099
layout: post
title: !binary |
  RmFjZWJvb2sg5a6j5biD6IiHIEhlcm9rdSDnmoTmt7HluqblkIjkvZzvvJrl
  haflu7rku6Xlj4rmm7TlpJroqp7oqIA=

date: 2011-09-16 17:54:26 +08:00
wordpress_url: http://blog.xdite.net/?p=3099
---
<a href="https://developers.facebook.com/blog/post/558/">Facebook Engineering Blog 公布了一則消息</a>，宣布與 Heroku 的更深度合作。

也就是在 Facebook 開應用程式時，可以直接選擇使用 <a href="http://heroku.com">heroku</a>。

標題不是猛料，猛料的在後面：

<h2> 直接內建 feature </h2>

根據該文，開通專案之後，會直接內建一個 example app。

app 內建 feature

* authentication
* displaying friends, photos, likes/interests, and friends who also use this app using the Graph API
* post to wall using the Feed Dialog
* send to friends using the Send Dialog

<h2> 支援 Python 與 PHP </h2>

一直以來，我們知道的是 Heroku 只支援 Ruby 以及 Node.js。但是在這次的合作中，也宣布了支援 PHP 與 Python。這應該是最讓人始料未及的 News...

<h2> 其他有趣的 Detail </h2>

我真的認真了去開了幾個專案來玩。發現幾件有趣的事：

1. 如果你原先就是 Heroku 用戶，若 FB 與 Heroku 的 email 一致，app 會直接開在該帳戶裡，而不另行通知。（我一直等不到信，跑去帳號裡面看才發現的）。
2. * Ruby 的 example app 是使用 <a href="http://www.sinatrarb.com/">Sinatra</a>
    * Python 的 example app 是使用 <a href="http://flask.pocoo.org/">Flask</a> + Jinja2( template language) 
    * PHP 的 example app 則沒有使用任何 Framework
3. 在寫這篇文章的時候，我還認真去找了一下 News，發現竟然沒人報導這件事情。<strong>如果 Heroku 支援 Python 與 PHP，那 PAAS 市場其他人大概就血流成河了吧</strong>...

# 寫這篇文章時的截圖，<a href="http://devcenter.heroku.com/articles/cedar">Cedar Stack 並沒有支援這兩種語言</a>：

<a href="http://www.flickr.com/photos/xdite/6152691310/" title="螢幕快照 2011-09-16 下午6.28.26 by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6071/6152691310_54bfc1ec96.jpg" width="387" height="500" alt="螢幕快照 2011-09-16 下午6.28.26"></a>
