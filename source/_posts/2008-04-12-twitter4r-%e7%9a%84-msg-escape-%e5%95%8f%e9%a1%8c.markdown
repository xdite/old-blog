--- 
wordpress_id: 569
layout: post
title: "Twitter4r \xE7\x9A\x84 msg escape \xE5\x95\x8F\xE9\xA1\x8C"
date: 2008-04-12 16:39:53 +08:00
wordpress_url: http://blog.xdite.net/?p=569
---
前幾天用 RoR 快速寫了一個取代 Twitthis.com 的程式（使用 Ruby 的 gem Twitter4r，後端加上了資料庫，做成了 Twitio.us。不過卻有使用者回報我的程式在送出 & 和 + 時有問題會跳掉。結果跑去 controller 修看看，escape 了半天都修不好。

結果後來才發現是 Twitter4r 本身的 code 這邊有問題

在 twitter4r-0.3.0/twitter/lib/twitter/ext/stdlib.rb
<code>
class Hash
  # Returns string formatted for HTTP URL encoded name-value pairs.
   def to_http_str
    result = ''
    return result if self.empty?
    self.each do |key, val|
-      result << "#{key}=#{URI.encode(val.to_s)}&"
+      result << "#{key}=#{CGI::escape(val.to_s)}&" 
    end
    result.chop 
end
</code>
就解決這個問題了。

-- 
解決這個問題的時候，因為程式所在的機器自己沒有 root，頂多只有 sudo gem 的權限，當然不能改 /usr/local/lib，於是就把 gem unpack 去 vendor/plugin。但是，但是……我卻忘了一件事，unpack 出來的目錄權限是 root:root ...超慘悲劇 orz。只好線上呼叫機器的 admin 幫我 chown XD
