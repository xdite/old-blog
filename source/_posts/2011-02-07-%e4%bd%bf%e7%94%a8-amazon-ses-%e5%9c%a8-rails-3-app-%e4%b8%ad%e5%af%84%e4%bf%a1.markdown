--- 
wordpress_id: 2005
layout: post
title: !binary |
  5L2/55SoIEFtYXpvbiBTRVMg5ZyoIFJhaWxzIDMgQXBwIOS4reWvhOS/oQ==

date: 2011-02-07 00:24:36 +08:00
wordpress_url: http://blog.xdite.net/?p=2005
---
在 <a href="http://blog.xdite.net/?p=1914">Web Startup 適合使用的服務與工具</a> 中，我提到了 Startup 可以儘量不要自己 build mail server，而改以第三方 mail service 取代，諸如<a href="http://madmimi.com/"> Madmimi</a> 或 <a href="http://sendgrid.com/">Sendgrid</a> 這一類的廠商。不過 AWS 最近更變本加厲的也推出類似的服務：<a href="http://aws.amazon.com/ses/">Amazon SES</a>。價格也十分的漂亮，簡直是海扁前述廠商...

Anyway，不過原先 Amazon 所提供與 Postfix 和 Sendmail 的整合方式，是叫起 <a href="http://aws.amazon.com/developertools/Amazon-SES/8945574369528337">Amazon Simple Email Service Scripts</a> 去整合。一整個看起來就不是很舒服啊....

Ruby 社群是個連寫 code 都講究美感的社群，這種東西怎麼可以包得這麼醜 XD。（我自己也覺得綁機器的設定...不舒服 XD）折騰了許久，大家相競寫出了幾個版本.....

最後 thoutbot 終於搗騰出了這一篇：<a href="http://robots.thoughtbot.com/post/3105121049/delivering-email-with-amazon-ses-in-a-rails-3-app">Delivering email with Amazon SES in a Rails 3 app</a>。一樣用 ActionMailer 的 API setting 就可以漂亮的搞定...

值得注意的是這個 gem 用了 ActionMailer 3.0.x 才存在的新 API : <a href="http://apidock.com/rails/ActionMailer/DeliveryMethods/ClassMethods/add_delivery_method">add_delivery_method</a> 才做出來的...。所以 Rails 2 App 通通哭哭啦 :/

----
Rails 工程師真苦命，幹新網站雖然比人家快了許多。但是隨著時間過去，很多好用的 gem 都只綁最新的架構，到了一定程度整個站還是要抽時間 upgrade 啊...
這時候就真的考驗平常 Code 和架構設計的乾不乾淨了...

乾淨的上天堂，骯髒的下地獄
