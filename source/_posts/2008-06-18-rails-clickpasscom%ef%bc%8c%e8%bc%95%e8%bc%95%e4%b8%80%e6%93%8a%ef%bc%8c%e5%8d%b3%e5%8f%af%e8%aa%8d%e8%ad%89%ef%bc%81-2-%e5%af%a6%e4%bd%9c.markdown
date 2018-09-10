--- 
wordpress_id: 594
layout: post
title: !binary |
  W1JhaWxzXSBDbGlja3Bhc3MuY29t77yM6LyV6LyV5LiA5pOK77yM5Y2z5Y+v
  6KqN6K2J77yBIC0gKDIpIOWvpuS9nA==

date: 2008-06-18 12:14:34 +08:00
wordpress_url: http://blog.xdite.net/?p=594
---
在之前 <a href="http://blog.xdite.net/?p=590">OpenID 架站懶人包一文</a>中，筆者提及了目前釋出的這個版本結合 <a href="http://clickpass.com">Clickpass</a> 相當方便。方便到其實你只要照著我貼的圖片打設定，就裝好了 XD。

1. 首先，當然是要把懶人包 on 起來，再至 <a href="http://clickpass.com">Clickpass</a>  申請帳號，切到 developer 設定。
( 筆者架了一個 demo site, <a href="http://opdemo.veryxd.net/">opdemo.veryxd.net</a>）

輸入這些資訊

<a href="http://www.flickr.com/photos/xdite/2589365452/" title="Flickr 上 xdite 的 圖片 2.png"><img src="http://farm4.static.flickr.com/3078/2589365452_a47c2794b2.jpg" width="500" height="318" alt="圖片 2.png" /></a>


2. 設定 OpenID 負責新增帳號的位置

以 demo site 為例，是 http://opdemo.veryxd.net/session

<a href="http://www.flickr.com/photos/xdite/2588530347/" title="Flickr 上 xdite 的 圖片 3.png"><img src="http://farm4.static.flickr.com/3194/2588530347_2c9e525d51.jpg" width="500" height="229" alt="圖片 3.png" /></a>

儲存設定之後，就可以把它提供的 button 語法貼到 /SITE/app/views/sessions/new.html.erb 裡面。

3. 設定 OpenID 負責整合帳號的位置

以 demo site 為例，是 http://opdemo.veryxd.net/users/add_openid

是 <a href="http://www.flickr.com/photos/xdite/2589365012/" title="Flickr 上 xdite 的 圖片 4.png"><img src="http://farm4.static.flickr.com/3050/2589365012_9c25962b11.jpg" width="500" height="236" alt="圖片 4.png" /></a>

底下的 callback url 也要設一下 http://opdemo.veryxd.net/users/add_openid。


<a href="http://www.flickr.com/photos/xdite/2589364844/" title="Flickr 上 xdite 的 圖片 5.png"><img src="http://farm4.static.flickr.com/3025/2589364844_1f16b703b1.jpg" width="500" height="165" alt="圖片 5.png" /></a>

儲存設定以後，將 button 語法貼到 /SITE/app/views/users/edit.html.erb 裡面。

這樣，就完成了 <a href="http://clickpass.com">Clickpass</a>  的整合，是不是很簡單呢？開始動手做一下吧！XD

如果有興趣的話，可以造訪敝公司的 <a href="http://sudomake.com">sudomake</a> 或我的 <a href="http://opdemo.veryxd.net">demo site</a> 試玩一下。


