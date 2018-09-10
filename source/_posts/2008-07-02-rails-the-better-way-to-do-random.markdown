--- 
wordpress_id: 598
layout: post
title: "[Rails] The Better Way To Do Random"
date: 2008-07-02 02:56:36 +08:00
wordpress_url: http://blog.xdite.net/?p=598
---
VeryXD 在週末升級完成了 Rails 2.1 之後，很高興的就在 PTT2 的個人版先發佈了這個消息。<a href="http://blog.gslin.org">大神</a>一時手癢就幫忙「測試」起敝小站，他測完之後在 blog <a href="http://blog.gslin.org/archives/2008/06/30/1532/">上寫了一篇關於 MySQL 調整的 tips</a>，大家可以參考看看。幫補了幾刀 index 調整以後大致上快了大概 5-6 倍吧 (Y)。

大神看完 MySQL LOG 以後覺得我在實作 random 的方式並不是很好，<a href="http://jan.kneschke.de/projects/mysql/order-by-rand">扔了這篇文章</a>叫我參考改寫。（P.S. 這篇文章的作者雖是 lighttpd 的作者，但他是 MySQL AB 的員工）

原先我在 Rails 1.2.6 的版本是用比較普遍的寫法。

用 Model.find("random") 的方式作。

[ruby]
  def self.find(*args)
    if args.first.to_s == "random"
      ids = connection.select_all("SELECT id FROM gphotos where status = 'published'")
      super(ids[rand(ids.length)]["id"].to_i)
    else
      super
    end
  end
[/ruby]

換到 Rails 2.1.6 時，我改用了 named_scope + rand。

[ruby]  named_scope :latest,:conditions => "status ='published'", :order => "id DESC"

  def self.pick
     self.latest.rand
  end
[/ruby]

兩種寫法都是先去 SQL 拉回來，在 app 這邊算，但是這樣效率也是不太好。

目前最新的版本照了<a href="http://jan.kneschke.de/projects/mysql/order-by-rand">文章建議的方式</a>改寫，把 rand 又扔回 database 方去做。
[ruby]
def self.pick
   return Gphoto.find_by_sql("SELECT * FROM gphotos as r1 JOIN (SELECT CEIL(RAND() * (SELECT MAX(id)     FROM gphotos)) AS gid) AS r2 WHERE r1.id >= r2.gid and r1.status = 'published' ORDER BY r1.id ASC LIMIT 1;").first;
end
[/ruby]

[ 先用找出一個值 CEIL(RAND*MAX(id)) 找出一個亂數值，但是這個亂數 id 可能不落在你希望的條件的範圍內，因此要取 id 大於此值但符合條件的下一筆資料）]

為何用 temp table 作 JOIN 而不用 id >= status 可以參照<a href="http://jan.kneschke.de/projects/mysql/order-by-rand"> 文章內的警告</a> 或 大神（<a href="http://blog.gslin.org/archives/2008/07/02/1535/">這篇寫的中文版解釋</a>）。至於 r2 為何要取名叫 gid，而不是 id ，是因為 join 出來的結果會有 2 個 id，然後 rails 就會精神錯亂 ....

再用 httperf 下去打，速度快了 30 倍（P.S. gphotos 大概是六萬多筆的資料量）。

提供這方面的實務數據讓大家參考一下。

