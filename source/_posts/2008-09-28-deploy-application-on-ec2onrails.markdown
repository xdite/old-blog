--- 
wordpress_id: 734
layout: post
title: Deploy Rails Application on EC2onRails
date: 2008-09-28 20:54:01 +08:00
wordpress_url: http://blog.xdite.net/?p=734
---
大概幾個禮拜前就學會怎麼使用 EC2onRails 了，只是有點忙碌，就忘記把步驟寫上來。

- 需要知識：
* 知道如何使用 <a href="http://www.capify.org/">Capistrano</a> deploy code 到 WebApp 上。
* 知道 EC2 是什麼，擁有 AWS 帳號
* 會使用 Elasticfox 開 AMI，熟悉 Elasticfox 的操作步驟。( 可以看之前筆者寫的這一篇 <a href="http://blog.xdite.net/?p=672">包 EC2 image 的 HOWTO</a>）

EC2onRails 是一塊 Ubuntu Linux server image，但並不是一塊乾乾淨淨的 image，應該說已經被特製成配合 ec2onrails 這個 gem，需要的時候可以迅速的 setup 一台 web application production 環境。preinstall 了相當多東西。

* 安裝 gem : sudo install ec2onrails
* 拷貝 <a href="http://github.com/pauldowman/ec2onrails/tree/master%2Fexamples%2FCapfile?raw=true">Capfile</a> 到 project 根目錄下 
* 拷貝 <a href="http://github.com/pauldowman/ec2onrails/tree/master/examples/deploy.rb">deploy.rb</a> 到 config 下
* 拷貝 <a href="http://github.com/pauldowman/ec2onrails/tree/master/examples/s3.yml">s3.yml</a> 到 config 下。

- 簡單解釋一下幾個比較重要的 option
[ruby]
set :application, "yourapp"  # App name
set :repository, "http://svn.foo.com/svn/#{application}/trunk"  # svn path
ssh_options[:keys] = ["#{ENV['HOME']}/.ssh/your-ec2-key"]  # keyfile 的路徑 ( Elastfox Keypair 產生的 Key )

role :web, "ec2-12-xx-xx-xx.z-1.compute-1.amazonaws.com"
role :app, "ec2-34-xx-xx-xx.z-1.compute-1.amazonaws.com"
role :memcache, "ec2-12-xx-xx-xx.z-1.compute-1.amazonaws.com"
role :db, "ec2-56-xx-xx-xx.z-1.compute-1.amazonaws.com", :primary => true
[/ruby]
# 把  AMI 開起來之後，可以拷貝 public DNS name ，就是貼到這裡
[ruby]
:packages => ["logwatch", "imagemagick"], # 填你需要的 application，會自動 apt-get install
:rubygems => ["rmagick", "rfacebook -v 0.9.7"], # 填你需要的 gem ，會自動 gem install
[/ruby]

- 設好之後就可以開始 deploy 到機器

* cap ec2onrails:get_public_key_from_server # 需要先 get server public key。
* cap ec2onrails:setup # 會把你要的環境統統裝起來，DB 也會開好。
* cap deploy:cold # 把 code 第一次 deploy 上去。

理論上這樣就完成了。

--- 
不過因為 ec2onrails 發展早於 Amazon EBS (Elastic Block Store) 的推出，因此 MySQL 必須透過定時備份到 S3 以及即時將 binlog 備份到非 Amazon 的站台以確保資料的安全性。ref from <a href="http://blog.gslin.org/archives/2008/08/21/1632/">DK 大神的  Amazon EBS (Elastic Block Store)</a>。

所以這兩個指令是必備的。

* cap ec2onrails:db:archive
* cap ec2onrails:db:restore

目前 EC2onRails 最新的 release 版本是 0.9.9，不管是 gem 或者是 AMI 都還未支援 EBS。
而<a href="http://github.com/pauldowman/ec2onrails/tree/master"> trunk 版</a>的已經透露會支援自動把 MysSQL 搬到 EBS，剛剛興致勃勃的跑去玩，玩到很後面才發現需要 0.9.10 的 AMI （多加了一些神秘的 setting 與 command )配合，而它目前是 private 的不給開 ~"~。

所以要碼就是等 0.9.10，不然需要就是自己開好 0.9.9 ，手動搬 ..

關於 0.9.10 的這部份，可以看一篇 blog <a href="http://blog.sweetspot.dm/tech-babble-using-amazons-ebs-with-ec2onrails/">Tech-babble: Using Amazon’s EBS with Ec2onRails</a>，不過它提到的 ami 我開不起來就是了 ...
