--- 
wordpress_id: 777
layout: post
title: relative_url_root is now deprecated after Rails 2.1.1
date: 2008-10-28 15:43:27 +08:00
wordpress_url: http://blog.xdite.net/?p=777
---
今天老闆將手上的 project 升到 <a href="http://guides.rubyonrails.org/2_2_release_notes.html">Rails 2.2</a>，就開始了我的踩地雷之旅。

原本 database.yml 可以用很寬鬆的寫法。username 可以寫成 user，現在不行。以前資料庫密碼可以純數字，會自動幫你 .to_s，現在也強制要括上 " "。 不然就會 TypeError。

值得注意的是，我手上的 project 大多有 support openid，裡面用的到 openid_authenicaion plugin 有一行會用到 relative_url_root。我一用 openid login 就噴掉了，google 了一下發現 relative_url_root 在 2.1.1 就 deprecated 掉了。

我暫時的解法是去 <a href="http://github.com/rails/open_id_authentication/tree/master">open_id_authentication</a> 抓新的版本回來，再打上這一隻 patch 解掉。

晚一點，我會去 patch 掉 <a href="https://github.com/xdite/openid_pack/tree">openid_pack</a> 以及 <a href="https://github.com/xdite/yahoobb_pack/tree">yahoobb_pack</a> 有關於這方面的 bug。
