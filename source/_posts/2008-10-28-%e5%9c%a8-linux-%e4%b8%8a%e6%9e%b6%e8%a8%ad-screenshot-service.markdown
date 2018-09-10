--- 
wordpress_id: 765
layout: post
title: !binary |
  5ZyoIExpbnV4IOS4iuaetuiorSBTY3JlZW5zaG90IFNlcnZpY2U=

date: 2008-10-28 12:21:24 +08:00
wordpress_url: http://blog.xdite.net/?p=765
---
前兩個禮拜公開的「<a href="http://govsucks.tw">民怨網</a>」這個 Service，眼尖的人應該有發現，民怨網是使用 <a href="http://ppt.cc/yo2/index.php">PPT.cc 提供的有圖有真相</a> API 服務對網站做了截圖快照的功能。

「有圖有真相」網是在跟 PPT 站長在喇賽時，提到我之前在做「<a href="http://award.veryxd.net">華文不及格大獎</a>」時，一直想加上幫部落格拍照的功能，但是 survey Linux Solution 時一直兜不出來，問他有沒有興趣一起解決這個問題，之後誕生的。他是使用了一套 Windows Software 截 IE 的圖，然後自己寫程式暴力呼叫幹出了「有圖有真相」網。

有強者寫出截圖 Service ，而且還有神秘的 API 可以神，當然是快樂的用 XD。

不過用歸用，我自己還是有股好勝心想挑戰這個謎題。因為不管怎麼樣，Pure Linux Solution 還是比較蘇湖啊 XD

基本的想法：手上只有一台向 <a href="http://linode.com">Linode.com</a> 租的 VPS 跑 Linux Server，我想嘗試在上面跑 Firefox 截圖達到我的目的。

（之前失敗是因為嘗試用 html2ps , ps2jpg 然後卡在莫名其妙的地方）

這是最後做出來的成果：

<a href="http://www.flickr.com/photos/xdite/2979085816/" title="Flickr 上 xdite 的 pixnet"><img src="http://farm4.static.flickr.com/3179/2979085816_c4a415a125_m.jpg" width="138" height="240" alt="pixnet" /></a> 

---
簡單記述一下我是怎麼做出來的：

我在 Linode 上面跑的 Linux 版本是 Ubuntu 8.04 Server。要在上面跑 Firefox 截圖，最大的問題是必須讓 Firefox 必須跑在 X 上。但前面已經說了這是沒有圖形裝置的 VPS，於是解法就剩下<strong>讓 Firefox 跑在 VNC 或 xvfb 兩種方式</strong>。

用 VNC 顯然是比較直覺的，因為可以用 VNC viewer 進去裝東西，怎麼想都比較快（但是我踩到了一堆 X11 相依的雷，最後放棄）。所以我用的是讓 Firefox 跑在 xvfb （虛擬的 X-window 環境）的方式。

安裝步驟 : 

1. (Clean) Ubuntu 8.04 Server
2. sudo apt-get install ubuntu-desktop
3. sudo apt-get install xvfb
4. sudo apt-get install netpbm
5. sudo apt-get install firefox-3.0

拍照方式:

[code]
 killall firefox Xvfb  
 Xvfb :2 -screen 0 1024x768x24 -fbdir /tmp -nolisten inet6 &  
 sleep 10  
 DISPLAY=:2.0 firefox -width 1024 -height 768 http://www.google.com &  
 sleep 10  
 xwd -display :2.0 -root -out shot.xwd  
 xwdtopnm shot.xwd > shot.pnm  
 pnmscale -xysize 200 150 shot.pnm > shotsmall.pnm  
 pnmtojpeg shotsmall.pnm > shot_thumb.jpg  
 pnmtojpeg shot.pnm > shot.jpg  
[/code]

這樣就能達到想要的網站截圖功能。（on Linux VPS !!!!!!）

不過到目前為止還沒有達到 100% 我要的功能，拍照當然是要拍全身的啊！

我又在 Firefox 上又裝了這個 Plugin：<a href="http://pearlcrescent.com/products/pagesaver/">Pearl Crescent Page Saver</a>，用 command 魂達到了這個目的 XD （ 我花 15 USD 買了 pro，因為 <strong>pro 版本在 command 列可以下支援更多參數</strong>）

到這一步的時候又會踩到幾個雷：

* <strong>把 Firefox 關掉再打開時，會跳出 Restore Session 的詢問選項</strong>，但是在 command 列上沒有滑鼠可以進去按確定，所以會卡住。要在 ~/.mozilla/firefox/xxxxx.default/prefs.js 裡加入這兩行叫他遇到同樣情形時閉嘴。
[code]
user_pref("browser.sessionstore.enabled", false);
user_pref("browser.sessionstore.resume_from_crash", false);
[/code]
* 用 command line 安裝 plugin xpi 時（ firefox -install-global-extension xxxx.xpi），又會跳出確定信任的詢問框需要按確定，<a href="http://blog.whirix.com/2007/05/screenshots-of-web-pages.html">我照了這一篇的指示要 Firefox 閉嘴</a>，但還是未遂。


最後的解法蠻 XD 的，因為耗費了太多時間在解決這個問題，搞到我生氣了，暴力將 windows 的 FF extension 目錄扔上去 linux 上，結果成功 XD。

大致上都搞定了，剩下最後的問題。就是要<strong>讓這個 Firefox 支援 Flash</strong>。但是 sudo apt-get install flashplugin-nonfree 拿到的是 Flash 區跑不出來的結果。於是<a href="http://www.ubuntugeek.com/how-to-install-adobe-flash-player-10-in-ubuntu-804-hardy-heron.html">我參照這一篇解掉了 flashplugin-nonfree 爛掉的問題</a>。

剩下的拉哩拉雜就是自己寫 daemon 設計 queue、API，不過這就不是本篇的重點了。

Anyway，在安裝上有任何問題歡迎留下你的 commant :)

----
延伸閱讀：

<a href="http://labnol.blogspot.com/2006/06/how-to-capture-save-screenshots-of.html">How to Capture & Save Screenshots of Webpages - Digital Inspiration</a>
<a href="http://pearlcrescent.com/products/pagesaver/doc/">Pearl Crescent Page Saver Documentation (Basic & Pro Editions)</a>
<a href="http://blog.whirix.com/2007/05/screenshots-of-web-pages.html">Whirix Technical Blog: Screenshots of Web Pages</a>
<a href="http://www.dslreports.com/forum/r19106630-Xvfb-Firefox-and-screenshots">Xvfb, Firefox, and screenshots - dslreports.com</a>
<a href="http://www.ubuntugeek.com/how-to-install-adobe-flash-player-10-in-ubuntu-804-hardy-heron.html">How to install Adobe Flash Player 10 in Ubuntu 8.04 (32 bit and 64 bit Hardy heron) -- Ubuntu Geek</a>
