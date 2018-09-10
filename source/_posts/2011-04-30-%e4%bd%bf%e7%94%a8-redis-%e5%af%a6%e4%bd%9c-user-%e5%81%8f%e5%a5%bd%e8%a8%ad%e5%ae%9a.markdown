--- 
wordpress_id: 2211
layout: post
title: !binary |
  5L2/55SoIFJlZGlzIOWvpuS9nCBVc2VyIOWBj+WlveioreWumg==

date: 2011-04-30 03:40:05 +08:00
wordpress_url: http://blog.xdite.net/?p=2211
---
<a href="http://t17.techbang.com.tw">T17</a> 最近<a href="http://t17.techbang.com.tw/topics/1774-notice-member-functional-upgrades">上了使用者偏好</a>這個功能。

<img src="http://cdn3.t17.techbang.com.tw/system/attached_images/2011/04/1781/show/aeb52e172f215f16f60c1b385bfebfdc.jpg?1303808838" alt="" />

傳統的方式是會使用 MySQL 實作 <a href="http://en.wikipedia.org/wiki/Entity-attribute-value_model">EAV</a>。不過這個功能我們是使用 <a href="http://zh.wikipedia.org/wiki/Redis">Redis</a> 作為 backend 實作的。

作法也很簡單，是採用了 <a href="https://github.com/nateware/redis-objects">redis-object</a> 這個 gem 將 Redis 的資料類型直接 mapping 到 Ruby object 去。所以我們可以直接把 Redis 的 key value 直接整進去 ActiveRecord 的 object 內作為一個 attribute。

以下就是程式碼，相當的簡單乾淨。form, controller , model 一行都不用改。

<script src="https://gist.github.com/948864.js"> </script>

==
credited: <a href="https://github.com/v1nc3ntlaw"> vincent@techbang</a>
