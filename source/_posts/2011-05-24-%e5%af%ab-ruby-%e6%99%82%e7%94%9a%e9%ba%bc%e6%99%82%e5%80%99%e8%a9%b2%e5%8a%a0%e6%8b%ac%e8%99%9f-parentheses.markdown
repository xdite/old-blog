--- 
wordpress_id: 2265
layout: post
title: !binary |
  5a+rIFJ1Ynkg5pmC55Sa6bq85pmC5YCZ6Kmy5Yqg5ous6JmfIChwYXJlbnRo
  ZXNlcyk=

date: 2011-05-24 02:01:21 +08:00
wordpress_url: http://blog.xdite.net/?p=2265
---
<p>之前在制定部門內寫 code 規則時，在大多數規則上同事都沒有甚異議。但最大的難處是甚麼時候需要使用 () ，這是個相當難拿捏的地方。這肇因在 Ruby 內，如果要 call method，用空白分隔，或者是用 () 傳遞參數，這是等價的。</p>

<p>最後定版的原則是：一率使用 ()，避免多 receiver 時被錯誤 parse。但 render , redirect_to , puts 這些習慣不用括號的地方可以不用。</p>

<p>最近在重讀 <a href="http://www.amazon.com/Eloquent-Ruby-Addison-Wesley-Professional/dp/0321584104">Eloquent Ruby</a>，讀到談這個主題的章節時，突然蹦出了一些想法。又跑去看了 <a href="http://blog.grayproductions.net/articles/do_i_need_these_parentheses">James Edward Gray II Blog 上的論戰</a>後，整理如下：</p>

<p><strong>預設能加 () 就加 ()</strong></p>

<p><strong>沒有 paramenter 時一率沒有 ()</strong></p>

<pre>
def words
  @content.split
end

def words()
  @content.split
end
</pre>

<p><strong>如果這是一句 statement 的話不加()</strong></p>

<p>如：</p>

<p>1. puts 等 I/O 句</p>

<p>2. render , redirect_to 等轉向句</p>

<p>3. if 的 statement</p>

<pre>

puts "Hello World"

redirect_to posts_path

if A > B

</pre>

<p><strong>如果這是個 statement，但句子裡面有多重 reciever的話必須加 ()，以確保斷句正常</strong></p>

<pre>
puts((1 +2) * 3) 

if obj.is_a?(Whatever) or obj.is_a?(WhateverElse)

</pre>

<p><strong>當然 super與 super() 是有差別的</strong></p>

<pre>

super without parentheses calls the method of the base class with all the arguments 
passed to the current method; 

super() instead calls the method of the base class without arguments.
</pre>
