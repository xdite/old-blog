--- 
wordpress_id: 1016
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE4\xB8\x89\xE7\xAB\xA0 Cache Expiration"
date: 2009-03-11 09:00:48 +08:00
wordpress_url: http://blog.xdite.net/?p=1016
---
這一集將討論的是 Expiration Strategy。

也許你還記得，上一集介紹了 update 的 action 裡，使用了 expire_page 去作 page 的 expiration。但如果只要會變動資料的 action 如: destroy 、create，都這樣暴力搞，頓時整支程式就會變得很髒。

<a href="http://www.flickr.com/photos/xdite/3336498930/" title="Flickr 上 xdite 的 vlcsnap-16586900"><img src="http://farm4.static.flickr.com/3368/3336498930_d73c6e0274.jpg" width="500" height="375" alt="vlcsnap-16586900" /></a>

有個方式可以簡化它：把 clear_posts_cache 寫成一個 method，再用 after_filter 呼叫它，這樣就會漂亮許多。

<a href="http://www.flickr.com/photos/xdite/3335664123/" title="Flickr 上 xdite 的 vlcsnap-16588245"><img src="http://farm4.static.flickr.com/3335/3335664123_effd854e24.jpg" width="500" height="375" alt="vlcsnap-16588245" /></a>

但問題又來了，也許不只 PostController 的這些 action 需要呼叫這個 method，其他 controller 如 Comment 也可能需要，那又要怎麼作呢？


<a href="http://www.flickr.com/photos/xdite/3336499072/" title="Flickr 上 xdite 的 vlcsnap-16589779"><img src="http://farm4.static.flickr.com/3348/3336499072_e164df2143.jpg" width="500" height="375" alt="vlcsnap-16589779" /></a>

有兩種作法：

1. 把 clear_posts_cache 放到 application.rb，讓所有 controller 都可以呼叫它。
2. 將 clear_posts_cache 變成一個 shared object ：把 clear_posts_cache 丟進 post_cache_cleaner.rb 讓 comments_controller 和 post_controller 去 include。

<a href="http://www.flickr.com/photos/xdite/3335671577/" title="Flickr 上 xdite 的 vlcsnap-16590999-crop"><img src="http://farm4.static.flickr.com/3314/3335671577_ef27cf63e5.jpg" width="500" height="150" alt="vlcsnap-16590999-crop" /></a>

但事實上我們可以不用這麼暴力，有一種更 convention 的方式已經內建在 Rails 裡了，稱之 Sweeper。Sweeper 不僅可以 Observes the Controller 更可以 Observes Models。
 
<a href="http://www.flickr.com/photos/xdite/3336499220/" title="Flickr 上 xdite 的 vlcsnap-16593011"><img src="http://farm4.static.flickr.com/3330/3336499220_234c5d86b0.jpg" width="500" height="375" alt="vlcsnap-16593011" /></a>

作法也很簡單：去 enviroment.rb 將此選項 uncomment 

[Ruby]
config.load_paths += %W(#{RAILS_ROOT}/extras  
[/Ruby] 
並修改成

[Ruby]
config.load_paths += %W(#{RAILS_ROOT}/app/sweepers
[/Ruby] 

在 app 下開一個新資料夾 sweepers，新建一個檔案叫 post_sweeper.rb。內容如下：

<a href="http://www.flickr.com/photos/xdite/3335664595/" title="Flickr 上 xdite 的 vlcsnap-16597583"><img src="http://farm4.static.flickr.com/3372/3335664595_cd81965b8f.jpg" width="500" height="375" alt="vlcsnap-16597583" /></a>

做好以後，也要在 controller 裡面設定哪些 action 才要呼叫 sweeper 。

<a href="http://www.flickr.com/photos/xdite/3335677005/" title="Flickr 上 xdite 的 vlcsnap-16596957-crop"><img src="http://farm4.static.flickr.com/3357/3335677005_9c83d2aae2.jpg" width="500" height="161" alt="vlcsnap-16596957-crop" /></a>

sweeper 可以使用任何 ActiveRecord 的 callbacks 。

<a href="http://www.flickr.com/photos/xdite/3336499554/" title="Flickr 上 xdite 的 vlcsnap-16597854"><img src="http://farm4.static.flickr.com/3622/3336499554_58d54f0239.jpg" width="500" height="375" alt="vlcsnap-16597854" /></a>

有些人其實不知道還可以用 controller callbacks，更還可以細到 action level。

<a href="http://www.flickr.com/photos/xdite/3336512570/" title="Flickr 上 xdite 的 vlcsnap-16598828-crop"><img src="http://farm4.static.flickr.com/3618/3336512570_6a5991170e.jpg" width="500" height="272" alt="vlcsnap-16598828-crop" /></a>

下一集將介紹的是此系列 Video 的贊助商：New Relic 的 RPM。
