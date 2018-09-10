--- 
wordpress_id: 1058
layout: post
title: "Scaling Rails - \xE7\xAC\xAC\xE5\x8D\x81\xE4\xB8\x89\xE7\xAB\xA0 Jim Gochee & Advanced RPM"
date: 2009-03-21 09:00:04 +08:00
wordpress_url: http://blog.xdite.net/?p=1058
---
Jim Gochee 是 New Relic 的 Director of Enginering。

Jim Gochee 也對 Scaling Rails Application 提出了三點建議：

1. Analyze your app in Production
使用工具如 New Relice RPM RPM 分析瓶頸在哪裡，從而改善。

2. Optimize your database use
DB 永遠是 Web Application 的瓶頸。做好 Cache，減少 Hit DB 的機會。善用 EXPLAIN 去搞清處 query 到底慢在哪裡。跟 ActiveRecord 混熟，學習讓它自動幫你產生比較有效率的 query。

3. Use ha_proxy with max-con 1
如果你的 Rails http server 是 mongrel，前面最好擺台 HAProxy ... 

---
後面又是 New Relic 的宣傳了，講 Silver 與 Gold Plan 的 Deatail，有些 Feature 是相當不錯的，不過我建議你直接看原 Video 吧 ...。
  
