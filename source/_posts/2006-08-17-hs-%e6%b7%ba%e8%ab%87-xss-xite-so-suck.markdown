--- 
wordpress_id: 67
layout: post
title: "[HS] \xE6\xB7\xBA\xE8\xAB\x87 XSS (X*ite So Suck)"
date: 2006-08-17 13:49:57 +08:00
wordpress_url: http://blog.xdite.net/?p=67
---
如果有在注意一些資安消息報導的讀者，應該會知道 XSS 是最近很紅的一個安全漏洞。XSS 的全名是 (Cross Site Scripting)，但是因為 CSS 已經被串接樣式表 (Cascading Style Sheets)先搶走縮寫了，所以就改成 X(cross)SS這個縮寫名，當然你高興<strong>解讀成標題</strong>那樣也可以 XD<br /><br /><br />因為各家網站都寫的差不多，剪貼資料很沒意思，所以就<a href="http://0rz.tw/cf1KZ">請你自行 google </a>囉。如果你悟性不錯又會寫 code 的話，看了幾個站，應該就懂得如何撰寫 exploit。比資料庫中出還要容易許多 ...<br /><br /><br />XSS 攻擊利用的，就是在頁面嵌入一段 script ，或者嵌入其他神秘的物件。只要有人一瀏覽此網站，就能擷取瀏覽人的 cookie （還可以用 script 做其他事）。而「餅乾」就相當於變身餅乾，只要誰吃到這塊餅乾就可以變成你。若是撿到管理員的餅乾，當然就可以變成管理員。變成管理員大家都知道可以幹嘛...:p 這就暫且不談了。<br /><br />變成另一個使用者那就非常有趣，XSS 通常倚賴的是釣魚手法，等那麼久好不容易才釣到一隻魚，簡直是浪費時間。所以強者如西毒歐陽鋒一樣，要殺掉海裡所有的鯊魚 ，就 .......（請翻閱射雕英雄傳），近來就有 <a href="http://0rz.tw/7523I">myspace 被 XSS 瞬間翻掉的例子</a>。<br /><strong><br /></strong><blockquote><strong>19歲的洛杉磯軟件開發員「Samy」編寫了一段蠕蟲程序，令他獲得了逾100萬網上「好友」，直至MySpace使該程序失效。他在自己的 MySpace簡介裡，置入一段JavaScript代碼，這樣每個查看簡介的人會在不知不覺中執行這段代碼。這段代碼把他列為該用戶的好友之一，而在通 常情況下，列為好友需要得到該用戶的同意，但他寫的蠕蟲使用Ajax技術，使之在後台批准他的請求。<br /></strong></blockquote><blockquote><strong><br />接著，該蠕蟲會打開該用戶自己的簡介，把惡意代碼復制進去，並把Samy添加到那裡的任何英雄列表中，還附上一句話：「但Samy是我最敬佩的英雄」。同樣，任何查看該用戶簡介的人也會被感染，這樣Samy的名聲和「人氣」迅速擴大到100萬MySpace會員。</strong><br /></blockquote>  <br />
<ul>
    <li>Q:那這種恐怖攻擊會不會在 Xuite 上發生？<br /><br />A:會，因為 Xuite 到處充斥著 Input Validation，可以說只要你夠有創意，隨便你愛怎麼玩都行。<br /><br /></li>
    <li>Q:有沒有辦法可以在瀏覽 Xuite 時阻擋這種手法？<br /><br />A: 沒有辦法。目前使用的主流瀏覽器，大概有兩家：<strong>M$的 IE</strong>、<strong>Mozilla 的 FireFox</strong>。<br /><br />IE 沒有讓使用者可以簡單關掉執行 script 的選項。它目前的只有防護 activex 選項的選項，又 java script 要打開所有的 activex 選項極其簡單 ....所以 IE 使用者只要打開 xuite 幾乎就等於肉雞任人打 XD<br /><br /> Mozilla 的 FireFox 則好一點，內定就可以使 java 不 enabled，還有外掛可以讓你選擇瀏覽此站是否選擇開啟 java script。聽起來好像看到救星，瀏覽 Xuite 用 FF 開就安全了。但是但是 ....Xuite 幾乎各項功能都是用 Java Script 寫出來的，關掉 Java Script 就等於什麼功能都不能用了。令人哭笑不得 XD<br /></li>
</ul>
<p><br />結論是，瀏覽 Xuite 的用戶只剩下兩個選項。不用 Xuite (不上站或是開FF自廢所有武功）或是用Xuite（當肉雞被隨便打）？<br /><br /><strong><font color="#ff0000">偉哉，XSS！</font></strong><br /><br />---<br />這還不是 Xuite 最嚴重的問題，下週本站將刊登更精彩的[HS]特稿。敬請期待。</p>
<p><font color="#ff0000"><strong>此系列僅提供安全資訊，請勿以身試法，本站將不負任何法律責任</strong></font></p>
