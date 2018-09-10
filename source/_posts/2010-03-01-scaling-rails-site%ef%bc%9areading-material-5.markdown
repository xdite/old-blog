--- 
wordpress_id: 1704
layout: post
title: "Scaling Rails Site\xEF\xBC\x9AReading Material # 5"
date: 2010-03-01 02:07:30 +08:00
wordpress_url: http://blog.xdite.net/?p=1704
---
6. <big><strong>Distributed Filesystem / Database , Database Optimization, NoSQL </strong></big>

基本上如果你的網站規模不到一定規模（這邊的定義是，用到幾十台機器），是不需要去研究 <strong>Distributed Filesystem / Distributed Database</strong> 的。不過我還是稍微聊一下它們的使用情境好了。



<blockquote><strong>Distributed Filesystem</strong> 我們在做完網站後，可以使用 Load Balancer 把 request 分散到各台 web-front 機器上，但隨之就會產生一個問題，全站上的靜態檔案要怎樣讓這麼多台的 web-front 存取呢？基本想法當然是用 NFS，但是隨著網站成長，很快的 NFS 就不夠擔當這種重責大任了。於是另一種簡單的作法又出現了，用大量的 rsync + script 將檔案複製到多台 來*暫時* 解決 NFS 不夠用的情況。這個方法勉強夠用，但是會隨之產生架構維護上的難度缺乏整體的管理與監控...。而且在有大量小檔案需要同步，或者是檔案數量到達了百萬級、千萬級時，純用 rsync 非常的傷...

所以這時候才會用上 Distributed Filesystem 來解決這個問題。DFS 的作用是讓 Application 不用理會檔案資源實際會存在哪裡，往裡扔就對了，接下來的事情 DFS 會自己處理掉。Opensource 中比較出名的 Distributed Filesystem 當屬 Hadoop Distributed File System 。

HDFS 想做到的是 Google File System (GFS）能提供的：

1. 易於擴充的分散式檔案系統
2. 可運作於廉價的普通硬體上，又可以提供硬體錯誤容忍能力
3. 給大量的用戶提供總體性能較高的服務

解決以上情景會遇到的問題，同時又提供擴充性、移植性、資料一致性，而且支援相當大的資料規模（Perabytes）。Facebook 本身也是用 HDFS。

有關於這部分的閱讀資料，可參考國網中心的 <a href="http://trac.nchc.org.tw/cloud/attachment/wiki/NCHCCloudCourse100127/1-4.pdf">Hadoop Distributed File System 這份投影片</a>。

而 <strong>Distributed Database</strong> 這個主題 <a href="http://blog.gslin.org/archives/2009/07/25/2065/">DK 講過也講的蠻清楚</a>了，我就不重複寫一遍了。最重要的目標是 CAP theorem 的 Eventual Consistent（資料最終一致性）。比較知名的 Distributed Database 有 HBase 、Cassandra、Voldemort 等等....</blockquote>


=====

<strong>Database Optimization</strong>

這方面的主題能談的非常多。而且每個主題也都相當博大精深。既然這系列的重點是 Reading Martrial，我就挑重點講好了。

首先是：<a href="http://blog.gslin.org/archives/2009/09/13/2088/">MySQL 的調校 (軟硬體、版本、設定)</a>，這一段 DK 也已經專門整理過一篇文章了。內容是有關於機器、硬碟的挑選，架構的設計。挑選適合 MySQL 使用的 FileSystem ，my.cnf 的調校。

而跟 MySQL 各方面都不是很熟，看原文書又覺得很痛苦的人。我推薦看對岸簡朝陽寫的 <a href="http://www.china-pub.com/195636">MySQL 性能調優與架構設計</a> 這一本書。基本上如果想瞭解 MySQL 的各樣基礎知識甚至進階知識，這本書大部分都含括了，而且寫的相當深入淺出....

架構設計上的需求，可直接看這本書的第三篇 (Ch12-Ch18)，以及 <a href="http://oreilly.com/catalog/9780596003067">High Performance MySQL</a> 這本書。追求極致的 Performance Optimizaion 可追 <a href="http://www.mysqlperformanceblog.com/">MySQL Performance Blog</a> 這個 blog ...

DB 永遠是 Web Application 的瓶頸。而使用 ActiveRecord 想對 query optimize 的幾個 tips 是：

<blockquote>1. 不要忘記打 index，這一點很常在寫 model code ，寫著寫嗨了就忘記了，忘記打 index 可能會造成 slow query，可用 <a href="http://github.com/eladmeidar/rails_indexes">rails_indexes</a> 這個 plugin。
2. 只 SELECT 你要的資料，而非 SELECT *。這一點可以透過 <a href="http://github.com/methodmissing/scrooge">scrooge</a> 這個 plugin 辦到。
3. 避免產生 n+1 query。
4. 記得加 counter_cache。（ 3. 與 4. 都可用 <a href="http://github.com/flyerhzm/bullet">bullet</a>  這個 plugin 抓出來 ）
5. 記得<a href="http://plog.longwin.com.tw/post/1/234">打開 my.cnf 裡面記錄 slow query log</a> 的選項，然後善用 EXPLAIN 抓出真正的原因。
6. 儘量使用 find_in_batch，而不要使用迴圈跑 find(i)
7. 少用 join，多用幾次 select 做到相同的事。
8. 必要的時候，使用 SQL Antipattern 技巧，denormalize，實作 eav 等等....這方面可以看 <a href="http://www.pragprog.com/titles/bksqla/sql-antipatterns">SQL Antipaatern</a> 這本書。
</blockquote>

=====
<strong>NoSQL</strong>

<blockquote>對岸 JavaEye 的站長 Robbin 寫過一篇「<a href="http://robbin.javaeye.com/blog/524977">NoSQL數據庫探討之一 － 為什麼要用非關係數據庫？</a>」。整理了為什麼世界上比較大型的 Web 2.0 Site 要捨棄 RDBMS 轉而開發/使用 NoSQL 的原因：

1. High performance - 對數據庫高並發讀寫的需求 
2. Huge Storage - 對海量數據的高效率存儲和訪問的需求
3. High Scalability && High Availability- 對數據庫的高可擴展性和高可用性的需求 

這些  production 網站，對 DBMS 特性的要求 ：

1.數據庫事務一致性需求 
2.數據庫的寫實時性和讀實時性需求 
3.對複雜的SQL查詢，特別是多表關聯查詢的需求 

以及市面上滿足這些需求的各種 NoSQL （依照分類）以及簡單介紹，相當值得一看。</blockquote>

另外 High Scalability 這兩天也出了一篇  <a href="http://highscalability.com/blog/2010/2/25/paper-high-performance-scalable-data-stores.html">Paper: High Performance Scalable Data Stores 
</a>

而我個人比較有在玩的是 MongoDB，他比較貼近 MySQL 的用法，但速度上快少 MySQL 不少。MongoDB 貼近 MySQL 的用法和一些其它的 feature，也比較滿足我在開發網站上的需求，比如說我就相當喜歡它以下的特色：

<blockquote>
* Document-oriented # BSON ( Binary JSON)
* schema-less
* full index support
* dynaminc query （<a href="http://www.slideshare.net/pengwynn/mongodb-ruby-document-store-that-doesnt-rhyme-with-ouch">MongoDB - Ruby document store that doesn't rhyme with ouch</a>P.34） 
* MM replication & auto sharding
* 可用來做 Real-Time Analytics ...# upsert 特性
* subdocument 甚至是 <a href="http://www.mongodb.org/display/DOCS/MongoDB+Data+Modeling+and+Rails#MongoDBDataModelingandRails-Nested%2CEmbeddedComments">nested document</a> # 做巢狀 comment 特別簡單。知名留言系統 Disqus （支援 Nested Comment ）的 backend 就是 Mongodb。
</blockquote>

2009 的 Rubyconf 就有一個關於 MongoDB 的 Talk ：<a href="http://rubyconf2009.confreaks.com/19-nov-2009-16-20-getting-non-relational-with-mongodb-michael-dirolf.html">Getting Non-Relational with MongoDB</a>，相當值得一看。 

====

以上就是我關於 Scaling Rails Site 的五篇文章，感謝收看...

另外推薦幾本 General Scaling 的書：
* <a href="http://oreilly.com/catalog/9780596102357">Building Scalable Web Sites</a> （Cal Henderson 著） # 這本是經典中的經典
* <a href="http://www.china-pub.com/195907">構建高性能 Web 站點</a> （郭欣 著）# 這是對岸出的新書，也是講了相當多 general topic

還有這一個 blog: <a href="http://highscalability.com/">High Scalability </a>
