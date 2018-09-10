--- 
wordpress_id: 1873
layout: post
title: "Amazon EC2 \xE6\x8E\xA8\xE5\x87\xBA micro instance"
date: 2010-09-09 17:33:30 +08:00
wordpress_url: http://blog.xdite.net/?p=1873
---
剛剛收到 AWS 寄來的通知信，摘要信件內容

<blockquote>
We are excited to announce the immediate availability of Micro instances for Amazon EC2, a new, low cost instance type designed for lower throughput applications and web sites.

Micro instances provide 613 MB of memory and support 32-bit and 64-bit platforms on both Linux and Windows. Micro instance pricing for On-Demand instances starts at $0.02 per hour for Linux and $0.03 per hour for Windows.
</blockquote>

重點就是<a href="http://aws.amazon.com/ec2/?ref_=pe_2170_16832160#pricing">一小時只要 0.02 美金</a>，一個月也只要 14.4 ( 0.02*24*30) 。比起 small 的 0.08 便宜不少。看起來用來做服務的 prototype server，或是放些小東西，都蠻合適的。

剛剛算了一下，<a href="http://www.linode.com/">linode</a> 這一類的廠商可能會被殺到。

<a href="http://www.flickr.com/photos/xdite/4973142323/" title="螢幕快照 2010-09-09 下午6.02.06 by xdite, on Flickr"><img src="http://farm5.static.flickr.com/4106/4973142323_9fa31429a0.jpg" width="251" height="258" alt="螢幕快照 2010-09-09 下午6.02.06" /></a>

以 512mb 的 solution，用 AWS 做是

14.4 ( EC2 micro 30 day) + 2 ( 200GB transfer) + 2.4 ( 16GB disk ) = 18.8
14.4 ( EC2 micro 30 day) + 3 ( 300GB transfer) + 3.6 ( 24GB disk ) = 21 

看起來我應該把 linode 停掉了。
