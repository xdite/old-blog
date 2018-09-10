--- 
wordpress_id: 829
layout: post
title: Facebook Connect on Rails
date: 2008-12-09 19:55:33 +08:00
wordpress_url: http://blog.xdite.net/?p=829
---
Facebook Connect 是 facebook 在今年五月推出的一項 feature，能讓使用者透過其他網站連結他們在Facebook上的帳號，亦提供使用者隨身攜帶其個人資訊，如基本資料、個人檔案圖片。 

引述 <a href="http://www.ithome.com.tw/itadm/article.php?c=52418">ithome 12/3 的相關報導</a> :

<blockquote>Facebook是在今年5月發表Facebook Connect，讓使用者透過其他網站連結他們在Facebook上的帳號，亦提供使用者隨身攜帶其真實的身份資訊，諸如個人基本資料、個人檔案圖片。姓名、朋友、照片、活動及群組等；此外，Facebook Connect開放開發人員存取用戶的社交網絡，即使使用者連到其他網站也能接觸Facebook上設定的友人。

此外，在隱私權部份，Facebook Connect提供使用者自行設定所要釋出的資訊，使用者亦可隨時移除列表上的友人。 </blockquote>

原先提供 Facebook Connect 合作的網站僅僅數家而已，但上週起 Facebook 宣佈 Facebook Connect 也開放所有的外部網站合作使用。

官方網站上提供了 <a href="http://developers.facebook.com/connect.php">php 版本的 example</a>。我也研究了一下做了一個 <a href="http://github.com/xdite/facebook_connect_demo_application/tree/master">Rails 版本的 example，放在 github 上，歡迎取用</a>。

* 特別需要注意的是你的 Callback URL 必須是 http://blablahbla.com/connect

* 這個 application 是我搞懂 <a href="http://spongetech.wordpress.com/2008/11/17/star-in-a-porno-with-facebook-connect-and-rails/">這篇文章</a>提到的 <a href="http://github.com/ckhsponge/zacknmiri/tree/master">Zack N Miri </a> 的運作之後，重新用最基本的 Restful Authenication + Facebooker 重新包一遍的<strong>***乾淨***</strong>結果。

如果你打算自己重新寫一個自己的 example 或 appliction，那麼有幾件事請務必注意。
1. layouts/application.html.erb ( 請務必 load 那幾份 js , facebook_require.js 與 application.js，還有 Feature Loader )
2. 在 public 下放置 connect/xd_receive.htm。( 這不是空的 htm，請不要自己 touch 一個 XD)
3. routes.rb，記得改寫 route。
4. 資訊取用的方式與平常操作 Facebooker 寫 Facebook Application 的方式無異。
