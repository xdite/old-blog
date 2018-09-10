--- 
wordpress_id: 803
layout: post
title: Merb-Auth HOWTO
date: 2008-11-10 07:17:27 +08:00
wordpress_url: http://blog.xdite.net/?p=803
---
前幾天 <a href="http://www.merbivore.com/">Merb</a> 1.0 在 <a href="http://rubyconf.org/">RubyConf 2008</a> 上釋出了。既然出了 1.0 ，看起來是可以進場的好時機。（雖然寫完這篇教學之後我的看法就改變了...）

於是週日接著繼續玩 Authenication 的部分。

Merb 在接近 1.0 的版本已經開始內建 <a href="http://github.com/wycats/merb/tree/master/merb-auth">Merb-Auth</a>。至於為什麼從 merful-authenication （類似 Rails 的 restful-authenication 的玩意）大變身成 merb-auth，這部分你可以參考 <a href="http://www.merbcamp.com/video">Merb Camp</a> 上 <strong>MerbAuth: Darwinian authentication</strong> 這一段的影片。

其實我還花了蠻多時間在研究怎麼使用，特別是為了要趕出 Merb 1.0 ，Merb 0.9.x - 1.0 這中間版本變遷所產生的雷多到簡直會讓人發怒翻桌 ....。然後 Merb-Auth 的 README 也相當沒誠意，看完了根本只知道怎樣 ensure_authenicated。要怎樣 signup/login 的部分通通沒有說 ...-_-|||。

<a href="http://github.com/RichGuk/merb-auth-example/tree/master">merb-auth-example</a> 這個 github 的 project ，翻完 controller 和 view 的 code 以後，我覺得他是來亂的 ...-_-

anyway。經過漫長的翻找文件以及 code 過程以後，我還是摸出怎麼實作的方式 ...

----


<blockquote>1. /login & /logout is default. so you don't have do anything. but in another hand, I don't know how to customize it's view . maybe someday I will figure out how to use it.

2. You can use /users/new to implement /signup ( write your own router.rb) . And when the user was be created ( /users/create action ) . make sure "session.user = @user".

3. Merb-Auth doesn't have logged_in? & current_user . So do it yourself .( you can copy some code from restful-authenication and modified it to fit merb)

4. If you need openid, you can read this project's code <a href="http://github.com/atmos/merb-openid-example/tree/master">merb-openid-example</a>.

Then you can build a basic authencation system on Merb .</blockquote>

