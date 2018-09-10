--- 
wordpress_id: 605
layout: post
title: "[Ruby] CSV to Hash"
date: 2008-07-24 01:14:39 +08:00
wordpress_url: http://blog.xdite.net/?p=605
---
這一小段程式是將 CSV 塞成 Hash 的暴力方式 ..。初衷是希望讀入 CSV 的第一行為各 Hash 的 KEY，其餘行為 VALUE。

也就是

name,number,country 
alice,1,TAIWAN
bob,2,USA
cindy,3,JAPAN

能被包成
[ { :name => "alice", :number => "1", :country => "TAIWAN" } ,{ :name => "bob", :number => "2", :country => "USA" },{ :name => "cindy", :number => "3", :country => "JAPAN" } ]

[ruby]
  require "csv"

   class << Hash
       def create(keys, values)
         self[*keys.zip(values).flatten]
       end
   end

  sp_array = Array.new

  first = true
  first_row = []

  CSV.open("XXX.csv","r"){|row|
    if first
      first = false
      first_row = row.to_a
    else
       result = Hash.create(first_row,row.to_a)
      sp_array << result
    end
}
[/ruby]

