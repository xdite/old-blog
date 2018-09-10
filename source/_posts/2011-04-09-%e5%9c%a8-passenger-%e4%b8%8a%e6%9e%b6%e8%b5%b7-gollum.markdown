--- 
wordpress_id: 2182
layout: post
title: !binary |
  55SoIFBvdyDmnrbotbcgR29sbHVt

date: 2011-04-09 22:22:23 +08:00
wordpress_url: http://blog.xdite.net/?p=2182
---
<a href="https://github.com/github/gollum">Gollum</a> 是 Github <a href="https://github.com/blog/699-making-github-more-open-git-backed-wikis">將他們的 wiki opensource 後再製的產品</a>，一言以蔽之：git-backend wiki。

原先 release 出來的版本，只能藉由 http server 跑在單一 port。如果想自己用 mod_rails 跑在 apache 上，需要寫大量的 dirty hack，結果還不一定 work。（gollum 剛出來時我就真的這樣幹了...)
剛剛想寫一點技術文章，又去把 Gollum 翻出來。弄一弄，發現翻到最新的解法還是不一定 work。

本來想放棄了。不過剛剛無聊讀了一下 RSS，看到 37signals 推出了新的 opensource project "<a href="http://pow.cx/">Pow</a>"。

Pow 是什麼呢？就是完全不用寫 config 的 rack server。利用 firewall rule 把 traffic 都導回 *.dev 。只要把 project symlink 到 ~/.pow 下，就可以直接把 project 開起來了..。
config 和 /etc/hosts 這種東西根本都不用改...

這實在太暴力了!!!

for example，我現在可以把我的 gollum  ln 到 ~/.pow 下，然後就可以開 http://wiki.dev 起來用了！實在太酷了...

btw, 這是 gollum 的 config.ru

<script src="https://gist.github.com/911737.js?file=config.ru"></script>

等一下我就要把它從 projects 搬去 Dropbox 了。這樣就可以 wiki anywhere 了。

===

* Mac 限定
* 如果發現一直找不到 gem 的情形，Pow 可能抓到是你 system 的 gem，強烈建議裝 <a href="http://rvm.beginrescueend.com/">rvm</a>，然後在 project 目錄下面使用 .rvmrc 指定版本...

===

update :

https://github.com/Rodreegez/powder 有 powder 這東西可以拿來管 pow
