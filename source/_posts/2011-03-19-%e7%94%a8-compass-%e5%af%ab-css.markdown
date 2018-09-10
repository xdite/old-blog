--- 
wordpress_id: 2097
layout: post
title: !binary |
  55SoIENvbXBhc3Mg5a+rIENTUw==

date: 2011-03-19 02:05:30 +08:00
wordpress_url: http://blog.xdite.net/?p=2097
---
最近在開發一個新產品，整體來說應該是接近寫完了，不過越接近完工，抓 IE 系列的 bug 就越是挫折。朋友 @evenwu 就來洗我要不要換成 <a href="https://github.com/chriseppstein/compass">Compass</a>，說這東西超神奇，超好用，還可以把 IE bug 殺光光！

其實之前就久仰 Compass 大名了，只是文件實在看起來太他媽的眼花繚亂，因為專案進度一直在跑，不太敢貿然換掉寫 CSS 的方式。而且怕配合的網頁美術跟不上。後來某天解票實在解到太賭爛了，下班後就花了一兩個小時摸這個東西，一用就驚為天人！

Compass = Sass framework powered by community。據 @hlb 用地球語解釋，programmer/designer 用了以後就不需要用知道怎麼做 haslayout/clearfix 的神器！

<a href="https://github.com/chriseppstein/compass">Compass</a> based on <a href="http://sass-lang.com/">Sass</a>。Sass 的作用是讓開發者可以用巢狀和寫程式的方式去撰寫 CSS。

用 Sass 開發有什麼好處呢？據我實際使用的情況 :

<blockquote>1. Sass 因為是以巢狀方式撰寫，再行 compile 成 CSS，所以<strong>不會再有 style 打架的問題</strong>

2. 寫錯（括號沒關, 色碼打錯字並不存在...等等）的話，Sass 會直接 compile 不過，註明第幾行 syntax error 或  color error ...。 （<strong>就不會發生找半天, 開了一堆 browser 還測不到靈異現象的原因</strong>）

3. Compatible CSS，所以舊的 style 直接貼上去也是沒問題的！

4. <strong>內建許多的 mixin</strong>，例如我最近在寫漸變顏色的效果，為了支援各家瀏覽器，我總要囉唆的寫上三行 CSS
<script src="https://gist.github.com/876513.js?file=gistfile1.css"></script>
不過換成 Sass 後，我就可以直接寫成 @include linear-gradient(color-stops(white, #cff2ff)); }，請參考<a href="http://compass-style.org/docs/examples/compass/css3/gradient/">這裡</a>。compile 過後它會自動幫我寫完那三行的 CSS

5. <strong>可以寫自訂的 mixin</strong>，上面的 mixin 不支援 IE，所以我還自己再寫了一個支援 IE 版，然後再 mixin 一遍。
<script src="https://gist.github.com/876519.js?file=gistfile1.sass"></script>
以後我要寫漸變色，就只要 @include my-linear-gradient(white, #cff2ff); 填入色碼就收工了。

6. community-based，意思是你可以去 github，找一堆人家寫好的<a href="https://github.com/imathis/fancy-buttons">一堆噁心超炫的特效</a>，想用直接 include 就收光了，不必辛苦寫老半天！
<img src="https://d3nwyuy0nl342s.cloudfront.net/img/5a4639cf1b760fa5009a2288058c631774dbc629/687474703a2f2f73332e696d61746869732e636f6d2f6465762f636f6d706173732f66616e63792d627574746f6e732f64656d6f2d6e6f2d6772616469656e74732e706e67" alt="fancy-buttons" />
7. 公司超省力，只要大家 html 和 sass 規則定好，以後做版不必再 ie hack 老半天（只要有人解過了就可以 include 來用）。一堆 RD 和一堆視覺設計師不需要再擔心誰跟誰搭有 style 不同的問題....。甚至視覺設計師以後只要專心做版即可，要是不小心 float 到爛掉就叫它直接 include self clear 就收工。</blockquote>

試寫過一輪以後超 high 的，本來卡在現有 CSS 已經一兩千行，重寫成本很高，沒想到 @hlb 又跟我說有 sass-convert （gem install sass --pre）這東西，我又更 high 了....馬上就開一個 branch 一口氣把 CSS 轉好。（自動掃描然後歸檔成 nested Sass）

至於 Rails 整合更方便，Compass 有<a href="http://wiki.github.com/chriseppstein/compass/ruby-on-rails-integration">直接整合 Rails 的 Solution</a>。討厭自己一直手動 compile，也有 compass watch，會一直幫你重新 compile。還有搭配的 gem 幫你解決掉部署問題。我現在都是開 compass watch + livereload 快樂的寫 html + css ...

<strong>Ruby (Web) Developer 真是太幸福了....</strong>

btw ，雖然 Compass 與 Sass 都是 Ruby 寫的，但如果你是用 Python 或 PHP 還是可以搭啦。只是這東西因為有 compile 的需要，部署上可能要自己研究一下方案。

---
update: @hlb 說 請愛用 sass alpha 跟 compass beta , 記得愛用 @extend, 少用 @include
