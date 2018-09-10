--- 
wordpress_id: 1791
layout: post
title: !binary |
  TGl2ZVJlbG9hZO+8muS9oOeahOWll+eJiOWlveW5q+aJi++8gQ==

date: 2010-07-15 16:37:20 +08:00
wordpress_url: http://blog.xdite.net/?p=1791
---
網頁設計師或者是程式設計師，無可避免的日常一定會遇到「切版」/「套版」的工作。做這些工作時，遇到最討厭的煩人事，除了「測各大瀏覽器相容性」，莫過於「邊切邊套按 Reload 重新整理瀏覽器整理到手抽筋」...。

<strong>要是檔案一變更，瀏覽器能自動更新就好了</strong>？

這個願望從前只有在 Firefox 上的 firebug （或者一些 js console）上能夠實現，而且也僅止於暫時修改 html/css/js 預覽結果的用途。

這個問題，最近被<a href="http://github.com/mockko/livereload"> LiveReload</a> 這個 rubygem 解決了！

在開始使用 LiveReload 前，你可以先觀看<a href="http://blog.envylabs.com/2010/07/livereload-screencast/">由 EnvyLabs 拍攝的示範影片</a>，了解使用方式。相信我，裝了它之後你就會愛上它了。

<object width="480" height="385"><param name="movie" value="http://www.youtube.com/v/EZ8vy_cNMVQ&amp;hl=zh_TW&amp;fs=1"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/EZ8vy_cNMVQ&amp;hl=zh_TW&amp;fs=1" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="480" height="385"></embed></object>

LiveReload 的特點有：


<blockquote>1. 更改 CSS 以及  JavaScript 類檔案時，存檔可以所見即所得（不需 reload page）。
2. 更改 HTML 或者其他 server-side script ( Ruby / PHP / Python 等等...），存檔時也可以所見即所得（瀏覽器會自動 reload）。</blockquote>



所以當你螢幕夠大時，一邊開瀏覽器一邊開編輯器寫 code 測試，搭配上 LiveReload，切版 / 套版速度會馬上大大的暴增！

有些人也許會好奇 LiveReload 的運作原理是什麼？


<blockquote>基本上 LiveReload 就是一個 directory watcher，透過設定工作目錄底下的 .livereload 檔下的檔案類型，執行 livereload 後，它會自動 watch 裡面的更動。它同時也自帶一個 http server，開發者安裝了它的 browser plugin ( Chrome / Safari supported )，client 與 server 連接起來。當目錄裡檔案更動時，browser 就自動 reload。</blockquote>


首次執行時不需要設定任何 .livereload 檔，執行後會自動產生...
預設會 watch 這一些類型的檔案。

<code>
Watching: /Users/xdite/projects/playgame
  - extensions: .html .css .js .png .gif .jpg .php .php5 .py .rb .erb
  - excluding changes in: */.git/* */.svn/* */.hg/*
</code>

如果你有其他的需要，可以修改 .livereload 檔，exclude 或 extend。

至於執行平台方面，Mac 當然是完全沒問題。它也提供了 Linux 與 Windows solution...不過這方面我就沒有測試過了，也許大家可以玩玩看 :D

如果都不能用，那就買台 Mac 加入使用 Mac 的行列吧！XD
