--- 
wordpress_id: 790
layout: post
title: Merb 0.9.12 CRUD example
date: 2008-11-04 01:01:27 +08:00
wordpress_url: http://blog.xdite.net/?p=790
---
因為 <a href="http://www.merbivore.com/">Merb</a> 即將推出 1.0，但目前還在難產。因此許多教學文件都處於相當混亂的狀態。這種狀態一直恐怕會一直延續到 1.0 Release 才會終止。

其實不只是文件，使用 merb-gen resource 生出來的 CRUD controller，竟然也是地雷，跟官方 tutorial 的 CRUD view 配在一起會炸掉（controller 的問題比較大）。所以...很多人想玩 Merb 其實是不得其門而入。

目前的 Merb 最新的版本是 0.9.12，我在這個版本的環境下，依照之前寫 Rails 的直覺和反覆的 debug，<a href="http://github.com/xdite/merb-blog-crud/tree/master">寫了一個 Blog have Post 的 CRUD example，放在 github </a>上。有興趣入門的人可以下載玩看看。
