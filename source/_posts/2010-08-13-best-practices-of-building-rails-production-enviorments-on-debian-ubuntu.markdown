--- 
wordpress_id: 1807
layout: post
title: Best practices of building Rails production enviorments ( on Debian / Ubuntu)
date: 2010-08-13 17:32:11 +08:00
wordpress_url: http://blog.xdite.net/?p=1807
---
這一篇其實是  <a href="http://blog.xdite.net/?p=1754">2010 Ruby on Rails 書單 與 練習作業</a> 中佈署篇的「官方版答案」。



<blockquote>* 租一台 VPS，挑選 Ubuntu 或 Debian 的 Latest 版本。
* 按照 <a href="http://github.com/jnstq/rails-nginx-passenger-ubuntu">http://github.com/jnstq/rails-nginx-passenger-ubuntu</a> 將整台環境架起來。</blockquote>

<strong>!! 需要特別注意幾點  !!</strong>
- Debian 有一些 command 需要特別 apt-get 安裝。查一下應該就有。雖然 Debian 比較麻煩點，但我還是推薦 Debian 而不是 Ubuntu，主要是些微的效能問題，Ubuntu 總是 heavy 一點。選擇 Debian / Ubuntu 是因為 package 比較新以及完整的原因。
- 絕對禁止 apt-get install ruby-full
- 注意 ImageMagick 必須依照指示用手動 compile 的。而非用 apt-get install imagemaigck。
- 注意 請每一步都照 recipes 上的指示安裝。不要異想天開跳過，否則後果自負..

接著

<blockquote>* 使用 <a href="http://www.capify.org/index.php/Capistrano">capistrano</a> deploy 專案。
 - 在 production 機器上開設 deploy 用帳號 apps，把自己的 public key 丟上 apps 這個帳號的 ~/.ssh/authorized_keys 裡。
 - 在 development 環境下對 project 下 capify . 
 - 編寫 config/deploy.rb 檔，這是 example: <a href="http://gist.github.com/522282">http://gist.github.com/522282</a>
 - cap deploy:setup, 然後上機器去設 shared/ 下的 config。（請注意，不要把 database.yml 等等具密碼的 config 也 commit 進專案，應設 .gitignore 迴避掉）。production 需要的 database.yml 請從 shared/ 下 ln 過去。</blockquote>



===

- 常見 Q & A

Q: <strong>為什麼不使用 FreeBSD / Linux 其他 distribution？而且看起來這套作法，與原先的 package system 脫節?</strong>
A: 有很多原因。舉一些例好了：

(1) <strong>[ gem 版本的問題 ] </strong>
FreeBSD 上提供的 ruby 與 rubygem 版本也許足夠純 ruby 開發使用。但對於 rails production 遠遠不夠，這是因為 rails 需要的一些關鍵工具，版本速度很快，而且往往需要相當新穎的 rubygem 版本才能夠使用。（ 如 Bundler 需要 gem 1.3.7+ 等等...）。但通常一些人使用 FreeBSD 的使用習慣通常是非用 ports 不可...

(2) <strong>[ 特定 gem 的地雷問題] </strong>
再來是，比如說中雷機率蠻高的 Rmagick 這個 gem，地雷很多。通常是用 OS package management  system 裝的 ImageMagick，再來裝 Rmagick，大概裝十次，有九次是完全不能跑的 !!!!! 而且，使用 apt/yum 安裝的 ruby-XXXX，通常是綁死原先的 apt/yum 的 dependency，越到後面這些相依性就會越來越混亂，混亂到無法收拾

(3) <strong>[特定 gem 版本綁死特定套件的版本]</strong>
比如說 xapian 這個 search engine，gem 版本號與 package 版本是一對一，互不相容啊  \_/。這時候用 pacakge system 就會 /_\

[[ <strong>不過就算我在這裡廢話這麼多，你也不會聽，等你中大獎才會覺得我中肯。習慣了...</strong>]]

所以實務上會建議，ruby 與 gem 完全與 package management system 脫節，至於特殊的套件安裝，用 <a href="http://www.rubyinside.com/chef-tasty-server-configuraiton-2162.html">chef</a> 去控管。37signals 幾個月前也釋出了他們的 <a href="http://github.com/37signals/37s_cookbooks">chef 的 cookbook</a>。

Q:<strong> 不過，這樣聽起來在最糟糕的情況下還是可能產生一股「無法用系統」管理的混亂？那怎麼辦...</strong>

A: 37signals 2007 年之後的作法，改採 deploy VM 的方式去做。與傳統佈署的方式想法不同，買來一台大機器，並不是一台灌一個 OS，而是一台切 8 個 VM，做 8 個一模一樣的環境。這樣雖然效能上會「有一點犧牲」，但是可以大幅減少「人工」（更新套件系統，處理 dependency 上的 effort）。畢竟「機器的價格」相較起「人的薪水」便宜太多了。

Q: <strong>為何要使用 capistrano？</strong>
A: 因為馬有失蹄，人有錯手啊。孩子...而且打個 cap deploy:rollback 就可以繼續回去睡，明天上班再修，那多好 ...

Q: <strong>為什麼要使用 ruby enterprise edition 和 mod_rails 啊？</strong>
A: 這次認真一點回答吧。ruby enterprise edition 吃的 ram 比較少，效率上也好一些。至於 mod_rails 是因為管理方便。again，機器比較便宜，人比較貴....

Q: <strong>為什麼要使用這個 recipes 啊?</strong>
A: 這是整個社群踩完雷之後累積的最佳實踐啊（淚）

Q: <strong>用 capistrano / chef / <a href="http://gembundler.com/">bundler</a> 有這麼重要嗎?</strong>
A: 有。它可以幫助你將傳統上沒有辦法進版本控制系統的「專案外部環境」，簡化到檔案等級，那麼就可以繼續用版本控制系統管理了。
 
Q: <strong>把所有東西進版本控制有這麼重要嗎，我不覺得耶？</strong>
A: 那是因為你還沒長大 -_-|||... 長大你就懂了。
