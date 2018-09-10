--- 
wordpress_id: 665
layout: post
title: Install RubyOnRails on Debian
date: 2008-09-13 15:56:46 +08:00
wordpress_url: http://blog.xdite.net/?p=665
---
最近在玩 <a href="http://www.amazon.com/gp/browse.html?node=201590011">EC2</a>，<a href="http://blog.gslin.org">壞人長輩</a>建議 OS 選擇 debian。
於是決定自己包一個 image for rails。

剛剛裝一裝套件，踩到一些雷，記在這裡提醒自己，也提醒其他人。
預計裝的有
<ul>
	<li>apache mpm worker</li>
	<li>mysql 5.1 server</li>
	<li>Rails 2.1</li>
	<li>Capistrano</li>
	<li>Mongrel & Mongrel Cluster</li>
	<li>Rmagick</li>
</ul>

安裝 apache & mysql 輕鬆寫意。

<code>apt-get install apache2-mpm-worker
apt-get install mysql-server</code>

但是之後，雷就一堆。因為接下來都是 gems ，所以要先裝 rubygems 這個管理工具。

 * 但是不要用 apt-get 上的 rubygems (0.95)，因為它是雷。透過裝的 rubygems 不會自動 link，直接 not found。實在懶得修，簡單的作法是是到 rubyforge 手動裝最新的 <a href="http://rubyforge.org/projects/rubygems/">rubygems</a> 1.2。

* gem install rails
* gem install capistrano
* 裝 Mongrel & Mongrel Cluster 之前要先 apt-get install build-essential，否則也會裝不起來。
* 裝 Rmagick 之前要先裝 ImageMagick，但是 apt-get install libmagick9 也會造成 Rmagick 裝不起來。因此閃雷的作法是到 ImageMagick 的官方網站抓 tarball 自己 make & make install 後，再 gem install rmagick。
