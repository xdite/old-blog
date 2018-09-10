--- 
wordpress_id: 609
layout: post
title: !binary |
  W1JhaWxzXSBhdHRhY2htZW50X2Z1IOWFp+mDqOaLt+iyneaqlOahiOeahOWv
  puS9nOaWueW8jw==

date: 2008-07-24 02:38:54 +08:00
wordpress_url: http://blog.xdite.net/?p=609
---
As we known, <a href="http://veryxd.net">VeryXD</a> 的上傳功能是使用 attchement_fu 這個 plugin 做出來的。

不過筆者當初在實做 VeryXD 時卻遇到了一個難題，官方的範例只講解了如何用 file upload 的方式丟檔案，沒有說如果要玩內部拷貝的話要怎麼作 <囧>，這樣是要怎麼不停的拷貝生圖啊，總不能合成的方式使用自己再丟 HTTP post 這種方式鳥方式塞進去吧。

所以當時花了一兩天在研究到底要怎麼內部扔 ...（從 Photo Model 扔到 Gphoto Model去），幾乎看遍了當時所有 Google 上能找到的所有資料 :(，還好最後是有暴力幹出來，不然就沒有 <a href="http://veryxd.net">VeryXD</a>了。

最近在寫 DEMOMO SHOW 用的比賽網站，又用到這招，找了一下還是沒有人提這個 trick，乾脆來貼一下要怎麼做好了。

[ruby]
      @photo = Photo.find(1)
      @award_photo = AwardPhoto.new
      @award_photo.content_type = @photo.content_type
      @award_photo.filename = @photo.filename
      @award_photo.temp_path = "#{RAILS_ROOT}/public"+@photo.public_filename
      @award_photo.save
[/ruby]

就這麼簡單，希望對大家有一點幫助。

