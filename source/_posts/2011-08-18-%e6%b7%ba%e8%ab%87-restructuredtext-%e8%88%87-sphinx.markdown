--- 
wordpress_id: 3020
layout: post
title: "\xE6\xB7\xBA\xE8\xAB\x87 reStructuredText \xE8\x88\x87 Sphinx"
date: 2011-08-18 05:18:41 +08:00
wordpress_url: http://blog.xdite.net/?p=3020
---
<a href="http://www.flickr.com/photos/xdite/6054176662/" title="sphinx.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6203/6054176662_ff8a30094c.jpg" width="300" height="52" alt="sphinx.png"></a>

<p>有朋友希望我談談這個主題。但因為最近非常忙碌，所以在這裡只能淺談，請見諒。</p>
<p>「 <a class="reference external" href="http://blog.xdite.net/?p=2998">第一次學 Rails 就上手</a> 」2011/8 月的版本包含了 PDF 版本以及 EPUB 版。
這次的版本不是改寫，而是「重新排版」。</p>
<p>這份書的初稿是用 Markdown 謄寫手排。而完稿則是用 Mac 上的軟體「Pages」手排。（你看，出書門檻不高吧 XD）</p>
<p>初稿編輯器是自己兜的（showdown + Rails backend )，讓自己能夠以很快的速度把書完成。</p>
<p>也許你會疑惑，為什麼我不用從頭用 Markdown 排版到底？</p>
<p>原因是：Markdown 不足以拿來出版書籍。</p>
<p>所以即便我是 Ruby Developer，最我還是選擇了 reStructuredText （python 開發者偏好格式）作為這次改版的主要格式。</p>
<div class="section" id="id1">
<h2>出版電子程式書籍需要面對的問題</h2>
<p>如果你想出版電子程式書籍。你將會遇到下列問題：</p>
<ol class="arabic simple">
<li>讀者希望出版商提供 PDF / EPUB / Mobi 三種無 DRM 版本。</li>
<li>必須在書中黏貼大量的程式碼以及加上 highlight。</li>
<li>必須張貼大量的示意圖片（可調整 Scale）。</li>
<li>必須有稍微複雜的表格。</li>
<li>除了基本的 Heading 、Ordered List，必須有 Warning / Note / Tips 以供參考。</li>
<li>如果章節過於冗長，必須能夠用巢狀文件書寫整理。</li>
</ol>
<p>在這情況下，Markdown 就完全不夠用了。</p>
<p>當然，你也可以使用 Markdown 的 extension，但是只要是 extension ，就不免落入各自為政的情況。這對即便是我這種有能力實作 <a href="http://markdown.tw/">Markdown </a> 出版系統的作者來說，也會是相當困擾的狀況。（介面的即時轉換，後端輸出成果）。</p>
<p>所以理想的狀況是，書籍的文件格式 <strong>必須在「基本格式」要能應付一定的複雜需求</strong> 。</p>
<p>最後我採用了 <a class="reference external" href="http://sphinx.pocoo.org">Sphinx</a>  + <a class="reference external" href="http://docutils.sourceforge.net/rst.html">reStructuredText</a> 。</p>
</div>
<div class="section" id="id2">
<h2>排版與成果</h2>
<ol class="arabic simple">
<li>支援 PDF 與 EPUB</li>
</ol>
<a href="http://www.flickr.com/photos/xdite/6054132580/" title="pdf-and-epub.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6085/6054132580_f3aaf8edd6.jpg" width="500" height="250" alt="pdf-and-epub.png"></a>
<p>PDF 為 latex template</p>
<a href="http://www.flickr.com/photos/xdite/6053582751/" title="pdf-screenshot.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6200/6053582751_5e361493b9_m.jpg" width="223" height="240" alt="pdf-screenshot.png"></a>
<p>EPUB 排程式碼也不錯看</p>
<a href="http://www.flickr.com/photos/xdite/6054132386/" title="epub-screenshot.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6075/6054132386_6e530850c7.jpg" width="375" height="500" alt="epub-screenshot.png"></a>
<ol class="arabic simple" start="2">
<li>可以黏貼大量的 console log  / 程式碼 以及加上高亮顯示</li>
<li>張貼圖片</li>
<li>複雜表格（我還沒排過故目前無沒示意圖）</li>
</ol>
<a href="http://www.flickr.com/photos/xdite/6053582389/" title="code-block.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6090/6053582389_b3a3ef31da.jpg" width="451" height="500" alt="code-block.png"></a>
<a href="http://www.flickr.com/photos/xdite/6054132256/" title="code-block-result.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6195/6054132256_aa567e6dc9.jpg" width="500" height="436" alt="code-block-result.png"></a>
<ol class="arabic simple" start="5">
<li>巢狀文件</li>
</ol>
<a href="http://www.flickr.com/photos/xdite/6053582515/" title="nest-document-1.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6190/6053582515_3eaa01ef43.jpg" width="500" height="247" alt="nest-document-1.png"></a>
<p>方便整理文件。可將文章或程式碼用 include 的方式嵌入。
在排版時不易黏膩糾纏</p>
<a href="http://www.flickr.com/photos/xdite/6053582571/" title="nest-document-2.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6194/6053582571_9a34750e1f.jpg" width="381" height="500" alt="nest-document-2.png"></a>
<ol class="arabic simple" start="6">
<li>Warning / Note / Tips</li>
<li>自帶 HTML 格式</li>
</ol>

<a href="http://www.flickr.com/photos/xdite/6054132134/" title="block.png by xdite, on Flickr"><img src="http://farm7.static.flickr.com/6197/6054132134_65be31f6d2.jpg" width="500" height="227" alt="block.png"></a>

</div>
<div class="section" id="id3">
<h2>是否為一般出版者能夠使用的工具？</h2>
<p>雖然 Sphinx 可以支援複雜的格式，滿足技術書籍出版的需求。
但相對的，它的技術需求對一般人也算高。</p>
<p>比如說：</p>
<blockquote>
<div><ol class="arabic simple">
<li>Sphinx 是純 command 操作，所以你必須要有 Unix 操作經驗</li>
<li>Sphinx 是 python 寫成，所以如果有特殊需求或要 debug，需要一定程度的 python 能力去修改核心。</li>
<li>PDF 是使用 latex 的 template 轉成，若轉出亂碼或想要更改排版格式需要一點程度的 tex 技巧。</li>
<li>PDF hook 的 tex 轉換引擎，你必須有能力知道怎樣掛上 command line 能夠讓 Sphinx 呼叫。</li>
</ol>
</div></blockquote>
<p>而 reStructuredText 目前也沒有算好用的 WSYSG editor。重排的作業我是在 <tt class="docutils literal"><span class="pre">vim</span></tt> 上排版完成的。</p>
</div>
<div class="section" id="id4">
<h2>結語</h2>
<p>重排這本書花了我總共 1.5 個工作天。
比用 Pages 排版快了一點點。</p>
<p>Pages 也沒什麼不好，我想也是一般人堪用的工具，但程式碼與程式碼造成的浮動 box 在 epub 成品上目前會有嚴重的錯亂問題。目前無解。</p>
<p>這是我目前摸到現在的經驗，提供大家做參考。</p>
<p>如果你對出版電子技術書籍有興趣，歡迎寫信切磋討論： xuite.joke AT gmail.com ，或者是 <a class="reference external" href="http://twitter.com/xdite">http://twitter.com/xdite</a> 找我也行。</p>
<p> 我的 <a href="http://gplus.to/xdite">Google +</a>、<a href="http://www.facebook.com/pages/xdite/372911557032">Facebook 專頁</a> 
<p>本文的 rst 原始版本在此: <a class="reference external" href="https://www.dropbox.com/s/cc6rwkn0pukn45v/01.rst">https://www.dropbox.com/s/cc6rwkn0pukn45v/01.rst</a> 希望對大家有幫助。</p>


====
<blockquote>

廣告：如果你有興趣看轉出來的全部成果，可以順道購買 <a href="http://rails-101.logdown.com/">書：第一次學 Rails 就上手</a> - 7 天之內學會 Rails（USD $9.99）

排書之餘也學學 Ruby on Rails （誤）
</blockquote>
