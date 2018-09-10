--- 
wordpress_id: 2067
layout: post
title: "Rails Performance Tuning (1) - \xE7\x94\xA8 select \xE6\x8F\x9B\xE6\x8E\x89 join"
date: 2011-02-27 01:57:29 +08:00
wordpress_url: http://blog.xdite.net/?p=2067
---
這系列是將以前一些實戰上的 tuning 筆記分批分享出來。每篇針對一個「常見情況」做出檢討與效能改進。

Rails 預設是使用 ActiveRecord 下 query。

所以我們會寫出這種 code

<em>@category.posts.published.limit(10)</em>

不過當 category 與 post 是 many to many 的情況下，就會產生 join 的情況。所以 controller 有這種 query 會很痛。

<blockquote> Post Load (271.4ms)   SELECT `posts`.* FROM `posts` INNER JOIN `post_categories` ON `posts`.id = `post_categories`.post_id WHERE ((aasm_state = 'published' and published_at <= '2011-02-26 17:54:19') AND ((`post_categories`.category_id = 1))) ORDER BY published_at DESC LIMIT 10</blockquote>

<strong>改進方式</strong>：

<em>@category.all_posts.published.limit(10)</em>

在 Category 多寫兩個 method，用兩次 select 換掉 join。
<script src="https://gist.github.com/845427.js?file=gistfile1.rb"></script>

<blockquote> PostCategory Load (3.7ms)   SELECT post_id FROM `post_categories` WHERE (`post_categories`.category_id = 1) 
  Post Load (14.5ms)   SELECT * FROM `posts` WHERE ((aasm_state = 'published' and published_at <= '2011-02-26 17:35:29') AND (`posts`.`id` IN (4,9,17,18,19,27,28,34,35,37,45,46,50,59,62,63,68,69,71,72,73,75,77,78,79,81,83,90,92,93,97,98,99...............) ORDER BY published_at DESC LIMIT 10
</blockquote>

速度會快上很多倍。
