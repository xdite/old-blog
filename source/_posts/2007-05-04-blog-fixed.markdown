--- 
wordpress_id: 338
layout: post
title: Blog Fixed!
date: 2007-05-04 15:12:13 +08:00
wordpress_url: http://blog.xdite.net/?p=338
---
Blog 修好了，繼昨晚大爆炸到修復這段過程，中間真是一言難盡。

昨天晚間七八點，我下班不久後，正在瀏覽自己 blog 時，突然 sql 就噴了。當時我不以為意，以為幾個小時就好，因為陸續幾天都有熬夜，身體上受不了，就先跑去睡了。

結果中間好像有讀者打手機來，似乎要跟我說 blog 壞掉的事，但是我真的很累，就沒有跟他繼續再講下去。僅僅向 bluehost report my db down，請他幫我修理。

但是當我大約 00:00 睡個七分飽醒來後，發現 blog 還沒好，這時我就詫異了。
打開信箱一看，bluehost 回信：

<blockquote>
Hello,

In looking at the issue that you are having I noticed that the account wasn't able to be verified.  You will need to contact us to verify the account.  Once the account is verified then you will be able to access the database.

Please let me know if you have any further questions.
</blockquote>我整個一頭霧水。不知道他在問什麼？什麼叫 verify account，我明明就在 open ticket 時輸入了我的 cpanel 密碼了啊。想了很久，再回去找找信箱，看看是不是有漏讀的 ...

結果我看到 4/27 有一封信被我漏掉。這是我在申請指第二個 A record 時對方的回信。

<blockquote>Dear Customer,

We need to verify that you are the actual owner of this account, please reply with either the password on the account or the last 4 digits of the credit card. Once we have this information we will process your request.

To check the server status please visits us at <a onclick="return top.js.OpenExtLink(window,event,this)" href="http://serverstatus.bluehost.com/" target="_blank">http://serverstatus.<span id="st" name="st" class="st">bluehost</span><wbr>.com/</a>
You can find the answers to most of your questions at <a onclick="return top.js.OpenExtLink(window,event,this)" href="http://helpdesk.bluehost.com/kb/" target="_blank">http://helpdesk.<span id="st" name="st" class="st">bluehost</span>.com<wbr>/kb/</a>

Thanks,
</blockquote>
所以，前後這兩封信對照起來，我以為是我沒確認身份，而在一週後導致我使用量最大的 database 被停用（因為其他 database 都能存取）。當下，我是非常憤怒的。我註冊帳號的 e-mail address 與 report address 都是同一個，而且筆者永遠使用 open ticket 的方式去提出回報（這是需要確認 cpanel 密碼的）。突然要確認我是不是本人，沒回就停我權，這算什麼？


為了要讓 blog 回來，於是壓著性子，回報了我的信用卡後四碼確認身份，然後跑去睡覺，希望清晨我再度醒來時，Blog 就好了。



早上七點多起床，站還沒修好（這是搞什麼）。但是我收到更莫名其妙的兩封信（因為我開了兩張 ticket）。

<blockquote>Hello,

In looking at the isssue I can see that the blog was just recently installed. &nbsp;Would it be a possible to reinstall the blog. &nbsp;If this isn't possible then the only other option would be to dump the database for the blog and then reinstall the program. &nbsp;Once the program is reinstalled then you could import the database. &nbsp;This would reflect your changes.
</blockquote>
----

<blockquote>Hello,

I have run the backup to restore your database from the daily backup. &nbsp;The issue still seemed to exist from the backup of the database. &nbsp;The only options would be for a back up of about a week ago. &nbsp;As stated in previous emails you may consider reinstalling wordpress and then importing the database for wordpress.

Please let me know if you have any further questions.
</blockquote>
到這邊已經是莫名其妙了。第一封信，先是問我他看到我最近 blog 安裝過（廢話，我才剛搬來不久，就裝那麼一次），如果不介意的話可以重裝 blog ，他會幫我把 db 倒出來再給我 import。第二封信則是說，他未及我同意，已經幫我把 database 的備份重倒進去，看看是否可 work，可是還是不行。如果這樣還是不行，最可能的方法就只可能取一週前備份，或是我選擇重裝 wordpress 再倒資料庫進去。


看到這裡我已經很生氣了，莫名其妙不能讀資料庫，還問我接不接受一週前的備份或者是昨天的資料庫但是 wordpress 重裝。


因為我自己完全無法 access 那個 db，phpmyadmin 一嘗試去開，就會死當。於是我請他寄一份給我，我自己試看看。


在等待他 dump 的同時，因為資料庫完全抓不回來。我採取把整個帳號資料打包全抓回來的方式，去抽 .sql 研究到底哪裡出問題。結果我在自己 freebsd 上跑 wordpress 再加上 dump 回來的 sql 完全沒問題 ...。但是把 bluehost 的 wp-config.php 接上 自己機器的資料庫就狂出問題...不是噴 cant' establish connection，就是吐 [an error occurred while processing this directive]，還有開 plugin，就跟我一直吐 header 問題的。


最後，我把 blog 重裝（yes，害我一些細部檔案要重寫），再開新資料庫把昨天整包備份裡的 sql 抽出來倒回去。竟然就正常了 -_-||||


靠邀，這不是擺明 bluehost 更改 php 設定，才害得我有問題嗎？不然好好的我沒動 blog，給我的建議竟然是重裝。而且在這篇文章寫完前，我跟他要的 dump db 還是沒給我。


翻桌 -_-||||


總之，blog 修好了，自己來還是快的多（怒）。

----
update:

good ，這篇寫完大概 20 分鐘後。我馬上就接到一封回信。

只寫了
<blockquote>
Dear Customer,
What happens when you try to download the file?
</blockquote>
