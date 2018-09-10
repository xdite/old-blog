--- 
wordpress_id: 2118
layout: post
title: !binary |
  QWN0aXZlUmVjb3JkIOeahCBzY29wZSDkuLLpgKPntLDnr4A=

date: 2011-03-22 15:00:44 +08:00
wordpress_url: http://blog.xdite.net/?p=2118
---
最近在 tune query

<blockquote>
scope :site_unhidden, where(["topics.site_id = ?", SITE_UNHIDDEN_IDS ])
scope :category_unhidden, where(["topics.category_id = ?", CATEGORY_UNHIDDEN_IDS ])
</blockquote>

當 model.site_unhidden.category_hidden 直接這樣串是會死人的。

MySQL 會直接噴 "Operand should contain 1 column(s) activerecord"

原因是 ORM 會下出這種 query 
SELECT SQL_NO_CACHE `model`.* FROM `model` WHERE (model.site_id = 1) AND (model.category_id = 1,2,3,4,5,6,7)

解法是 ? 旁邊都要包個單引號，變成 '?'。這樣就沒事了...

=====

update: 後來內部討論後，決定都改用 scope :site_unhidden, where(:site_id => SITE_UNHIDDEN_IDS) 這種方式寫，以免因為 ? 的括號問題踩到雷
