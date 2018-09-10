--- 
wordpress_id: 1974
layout: post
title: Code commit policy ( using Git )
date: 2011-01-15 07:01:39 +08:00
wordpress_url: http://blog.xdite.net/?p=1974
---
在<a href="http://blog.xdite.net/?p=1914">上一篇</a>，提到了我其實寫了不少 internal document （自己的一整套 Starter Guide, Code Convention, Commit Policy, Best Practice ）。

我設計的 commit policy 其實是這樣的。

<blockquote>

1. 所有站台 deploy 到 production 的 code 一率吃 master。

2. 大規模的改版會從 master 裡面 branch 出一個 staging 來持續開發。staging 開發完成沒問題之後才會 deploy。

3. 已經上線的 code 除了改 wording 之類的票，一率不准直接改 master。必須要另外弄一個 branch 出來做，這個 branch 強制吃票號，比如說 ticket 是 1234，則 branch 為 t1234。( 推上 remore）做完 peer review 沒問題之後才能 merge 回 master / staging，然後 deploy。

4. 稍微大一點的功能允許自訂 branch 名稱，比如說整個前台升 rails3，則可以開一個 rails3 branch。

5. 採取 meta commit 原則，可以是一個 method，一個 controller，一個 model 作一次 commit。確保 code review 能夠順利的進行。極度不鼓勵改 10 次檔案才 commit 一次，除非這是一張票，內容就是改這 10 個檔案的某個相同處。

6. 牽扯到 DB schema 變更的 commit / push 或太大規模的改寫，必須在公開頻道喊過務必確保同事知情。

7. * 絕對禁止 push 未完成（同事一拉下來結果爛光光）的 code，必須自己「手動」檢查過 code（如果有寫 test 就要 run test）才可 push。
    * 絕對不能用假想的方式修完 code 就 commit push 甚至 deploy。
    * 絕對禁止下班前一個小時 push + deploy。

8. (5) 與 (6) 累犯者(超過 3 次者) 踢出團隊，拒絕合作。

</blockquote>

有幾點需要特別解釋為甚麼要這樣設計。



<blockquote>1. 曾經有試過 production 與 master 並行，然後從 master 裡 cherry pick 回去 production。後來發現的問題是懶 RD 修 bug 有時候會直接 commit 在 production，而且 cheery pick 回去本來就比較複雜，加上這種情況，事情就會變得很複雜。後來重新思考過，主幹跑 master 再加上 ticket branch 事情就會變得簡單很多。

2. staging 加上 ticket branch 也很好丟上 staging 環境測有沒有問題...

3. RD 寫 code 一率 peer review，除了可以早期發現 junior RD 寫 code 的問題，meta commit 也可以讓 RD 習慣 一次單純只處理一件事，處理太多事一心多用寫出來的 code 會很糟糕。

4. 全域的變更要通知大家「不是一種禮貌」，而是「不通知亂炸大家就叫不禮貌」

5. 嚴格禁止 commit 爛掉的 code 是因為一炸就是浪費 *所有人* 的時間。自己炸自己就算了，炸大家的人最不能夠原諒。

6. 接近下班時間寫出來的 code 品質最糟糕（肚子餓+想回家）。更麻煩的是閃了晚上如果真的爛了就找不到人。所以下班前一小時禁止 push / deploy。

7. 做錯事而且屢勸不聽的人不處理那就等於虐待沒做錯事的人。而且一天到晚炸 code 的人，其實就是一顆對 business 的不定時炸彈。沒事為甚麼有人想養炸彈在家裡...</blockquote>

