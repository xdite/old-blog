--- 
wordpress_id: 2518
layout: post
title: !binary |
  54K65LuA6bq8IFJ1Ynkg6YGp5ZCI5ou/5L6G6KO95L2cIFJhaWxzIOmAmeao
  o+eahCBGcmFtZXdvcms/

date: 2011-06-26 22:58:12 +08:00
wordpress_url: http://blog.xdite.net/?p=2518
---
這是<a href="http://blog.xdite.net/?p=2407">上 Owning Rails 這門課</a> 抄下來的筆記。

解釋了為何  Rails 這樣的 Framework 是由 Ruby 做出來的，Ruby 具有以下幾項 feature，讓它非常適合創造有 nice API 的 frameworks

### Block

<pre>
before_save { |user| user.hug_and_kiss! }

Application.routes.draw do 
  resources :users
end
</pre>

### Extending core classes

<pre>
class String
  def pluralize
    self + "s"
  end
end

"user".pluralize
</pre>

### Super Sweet Syntactic Sugar

<pre>
has_many :roles, :dependent => :destroy
# instead of 
self.has_many(:roles, {:dependent => :destroy})
</pre>

### Every Thing is an Object, Everything has a Class ###

#### Everything is an object

<pre>
1.to_s # => 1
x = 1 + 2 # => 3
</pre>

#### Everything is also method send
<pre>
1.send(:+,2) # => 3
1.+(2) # = > 3
</pre>

#### Everything has a class

<pre>
"hi".class # => String
String.class # => Class
Class.class # => Class
</pre>

### Module & Extend

<pre>

class Pony
  
  def eat  
  end
  
  def self.eat_it_all!
  end
  # because self = Pony in this class
  
end

Pony.new.eat
Pont.eat_it_all!


module Vegetarian
  def eat_meat?
    false
  end
end

class Pony
  include Vegetarian
end

Pont.new.eat_meat?

s = "string"

s.extend Vegetarian
s.eat_meat?


class Pony
  extend Vegatarian
  # self = Pony
  # self.extend Vegatarian = extend Vegatarian
end

</pre>

因為具有 nice sugar syntax，所以我們可以這樣寫

<pre>
module ActiveRecord
  class Base
    def self.has_many(associaltion, options = {})
      p [associaltion, options]
    end
  end
end

class User < ActiveRecord::Base
  has_many :comments, :dependent => :destroy
  # equals => self.has_many :comments
  # equals => User.has_many(:comments,{ :dependent => :destroy })
  
end

</pre>

再來談談 blocks

<pre>

my_proc = proc do 
  puts "yeaaaah!"
end

puts "will it run?"

</pre>
執行結果：
<pre>
"will it run?"
</pre>
<hr>
<pre>
my_proc = proc do 
  puts "yeaaaah!"
end

puts "will it run?"

my_proc.call

</pre>
執行結果：
<pre>
will it run?
yeaaaah!
</pre>

<hr>
<pre>
def hug
  print "{"
  yield
  print "}"
end

hug { print "me" }
</pre>

執行結果：
<pre>
{me}
</pre>

<hr>

<pre>
def hug(&block)
  p block
  print "{"
  yield
  print "}"
end

hug { print "me" }
</pre>

執行結果：
<pre>
# &lt;Proc:0x00000001001393f8@untitled:8&gt;
{me}
</pre>

<hr>

所以我們可以這樣做出 Route

<pre>
class Route
  def initialize
    @routes = {}
  end
  
  def match(options)
    @routes.update(options)
  end
  
  def routes(&block)
    instance_eval(&block)
    p @routes
  end
end

Route.new.routes do 
  match "/users" => "uses#index"
  match "/login" => "sessions#new"
end

</pre>

執行結果

<pre>
{"/users"=>"uses#index", "/login"=>"sessions#new"}
</pre>

解釋

<pre>

class Route
  def initialize
    @routes = {}
  end
  
  def match(options)
    @routes.update(options) # Hash update => merge hash
  end
  
  def routes(&block)
    instance_eval(&block) 
    # do these things
    # match ( {"/users" => "uses#index" } ) 
    # match ( {"/login" => "sessions#new"} ) 
    p @routes
  end
end

Route.new.routes do 
  match "/users" => "uses#index"
  match "/login" => "sessions#new"
end

</pre>

====
<blockquote>

廣告：
<a href="http://rails-101.logdown.com/">書：第一次學 Rails 就上手</a> - 7 天之內學會 Rails（USD $9.99）
<a href="http://registrano.com/group/rubytaiwan">聚會：Rails Tuesday</a> Rails 社群聚會 （免費）2011/06/28
</blockquote>
