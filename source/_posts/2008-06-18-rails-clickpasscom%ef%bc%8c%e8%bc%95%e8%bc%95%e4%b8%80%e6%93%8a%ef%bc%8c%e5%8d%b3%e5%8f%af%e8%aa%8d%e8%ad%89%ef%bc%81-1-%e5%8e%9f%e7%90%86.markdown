--- 
wordpress_id: 593
layout: post
title: !binary |
  W1JhaWxzXSBDbGlja3Bhc3MuY29t77yM6LyV6LyV5LiA5pOK77yM5Y2z5Y+v
  6KqN6K2J77yBIC0gKDEpIOWOn+eQhg==

date: 2008-06-18 11:39:35 +08:00
wordpress_url: http://blog.xdite.net/?p=593
---
前幾天，筆者在 blog 上釋出了 <a href="http://blog.xdite.net/?p=590">Rails 的 OpenID 開站懶人包</a>。今天又將要釋出 <a href="http://sudomake.com">sudomake.com</a> 上另一個有趣的認證機制的做法，Rails 如何結合 ClickPass。

(如果讀者不清楚 OpenID 是何物的話，可參照<a href="http://briian.com/?p=5328">重灌狂人對於 OpenID 的詳細介紹</a>，或者是 <a href="http://blog.ericsk.org/archives/858">ericsk 對於 OpenID 認證機制的介紹</a>）

引述重灌狂人文章的一小段作為開場

<blockquote>

大家可能常會遇到一種狀況，每次發現一個好用的網站時，都得在該網站註冊一組帳號、密碼才能開始使用，時間久了之後，常常會忘記自己在哪個網站使用過哪個帳號跟密碼。雖然有些人會直接在各大網站使用同一組帳號、密碼，不過如果遇到壞心的站長或站務人員把你的帳號、密碼拿去別的網站登入、窺探你的秘密的話，那就防不勝防了。

 

後來，網路上漸漸的發展出一種OpenID的機制，所謂的OpenID就是一種共享式的帳號登入、識別系統，我們可以先在各大「OpenID註冊中心」免費註冊一組專屬的帳號，然後拿這個OpenID來登入其他「支援OpenID登入的網站」。

透過OpenID的標準化獨立認證、登入方式來登入各大網站，對於使用者來說只要註冊一次OpenID的帳號，就可以在每個支援OpenID登入的網站來去自如，不用再重新註冊或管理多組帳號、密碼，看起來方便多了。 </blockquote>

不過，即使網站有提供 OpenID 機制，但 OpenID 網址往往落落長一串，老是要打那麼多字真的是很煩。這時候我們想到了提供另一種方案，<a href="http://clickpass.com">Clickpass</a>！

<a href="http://clickpass.com">Clickpass</a> 也是一家 OpenID 提供商，完整的 IDEA 可參照這個影片！

<object width="400" height="300">	<param name="allowfullscreen" value="true" />	<param name="allowscriptaccess" value="always" />	<param name="movie" value="http://www.vimeo.com/moogaloop.swf?clip_id=883437&amp;server=www.vimeo.com&amp;show_title=1&amp;show_byline=1&amp;show_portrait=0&amp;color=&amp;fullscreen=1" />	<embed src="http://www.vimeo.com/moogaloop.swf?clip_id=883437&amp;server=www.vimeo.com&amp;show_title=1&amp;show_byline=1&amp;show_portrait=0&amp;color=&amp;fullscreen=1" type="application/x-shockwave-flash" allowfullscreen="true" allowscriptaccess="always" width="400" height="300"></embed></object><br /><a href="http://www.vimeo.com/883437?pg=embed&sec=883437">How Clickpass works</a> from <a href="http://www.vimeo.com/user413683?pg=embed&sec=883437">Peter Nixey</a> on <a href="http://vimeo.com?pg=embed&sec=883437">Vimeo</a>

而究其原理，<a href="http://clickpass.com">Clickpass</a> 在提供 OpenID 時的做法與較提供商較為不同，<a href="http://clickpass.com">Clickpass</a>  提供的 OpenID URL 是亂數產生的，而提供給每個網站的 OpenID URL 也都不一樣(還是會對應到同一會員，<a href="http://clickpass.com">Clickpass</a>  會判斷）。而使用者一但註冊成 Clickpass 的會員之後，只要到任何提供 <a href="http://clickpass.com">Clickpass</a>  登入的合作網站，只消輕輕一擊，就可登入，完全不用再打那一串落落長的 IDENTITY URL，相當方便省事。( 個人覺得很酷就是了 ...)

目前已經有數個比較有名的網站提供 <a href="http://clickpass.com">Clickpass</a>  的 Login 服務，如 <a href="http://Scribd.com">Scribd.com</a>、<a href="http://ma.gnolia.com">ma.gnolia.com</a>，甚至你還能在 Wordpress 上使用 Clickpass（有提供 wp-plugin)！
延伸閱讀；

和多官方網誌；<a href="http://handlino.com/blog/2008/06/10/69/">淺談 OpenID</a> 
