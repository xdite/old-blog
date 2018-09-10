--- 
wordpress_id: 2604
layout: post
title: "THE BETTER WAY TO ORDER BY RAND in scope "
date: 2011-07-04 23:31:30 +08:00
wordpress_url: http://blog.xdite.net/?p=2604
---
We often use rand functions in our websites. There are many ways. Some use the ORDER BY RAND() in SQL, or query all the data and shuffle it,(they all bad) blahblah....

When I try to turn-up performance for my company website today, I have found there are some slow pages.  They all have the same problem : bad performance cause by rand(). 

I remembered I had wrote a better version of rand() 3 years ago. Gslin taught me <a href="http://blog.gslin.org/archives/2008/07/02/1535/mysql-%E7%9A%84-order-by-rand-%E7%9A%84%E6%9B%BF%E4%BB%A3%E6%96%B9%E6%A1%88/">one tips to replace terrible "ORDER BY RAND()"</a> in that time.

But the original solution I wrote was "find_by_sql" version. That's because there are not such things like "scope" in Rails 3 years ago.

( Nick Kallen invented "has_finder" , then became "scope" in Rails 3. )

In 2011, we should write "scope" version everywhere. So I spent couple of hours to dive in.

The original SQL statement was found from the article <a href="http://jan.kneschke.de/projects/mysql/order-by-rand/">ORDER BY RAND()</a> by <a href="http://jan.kneschke.de/"> Jan Kneschke</a>.This article explains a lot about his alternative ORDER BY RAND solution.

This is the gist I wrote about scope version, and the experiments I already done.

<script src="https://gist.github.com/1063442.js?file=post.rb"></script>

Using this scope, you can write this 
<pre>
 Post.rand.published.limit(5)
</pre>

to get random data in less 1 ms.

But be caution, in many to many situation, like game has_many posts, through game_posts, with order scope, for example:

<pre>
 game.posts.rand.published.limit(5)
</pre>

It will become very slow ( 148ms, Data size is about 9400 ).

So we SHOULD use this way to query :

<pre>
Post.rand_by_game(3).published.limit(5)
</pre>

And one thing to mention, DO NOT order in this scope first, it will slow too.


===
<blockquote>
Advertise：
<a href="http://rails-101.logdown.com/">Book: Rails 101 - Learn Rails in 7 days</a>（USD $9.99）
<a href="http://registrano.com/group/rubytaiwan">Ruby Taiwan：Rails Tuesday</a>（Free）
</blockquote>
