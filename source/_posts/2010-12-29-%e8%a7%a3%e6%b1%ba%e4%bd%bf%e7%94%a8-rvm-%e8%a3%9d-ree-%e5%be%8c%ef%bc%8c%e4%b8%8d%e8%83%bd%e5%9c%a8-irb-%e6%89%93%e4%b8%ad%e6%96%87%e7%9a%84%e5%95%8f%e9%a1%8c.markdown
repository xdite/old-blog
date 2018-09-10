--- 
wordpress_id: 1903
layout: post
title: !binary |
  6Kej5rG65L2/55SoIHJ2bSDoo50gcmVlIOW+jO+8jOS4jeiDveWcqCBpcmIg
  5omT5Lit5paH55qE5ZWP6aGM

date: 2010-12-29 15:13:15 +08:00
wordpress_url: http://blog.xdite.net/?p=1903
---
<a href="http://nruth.tumblr.com/post/858778765/fixing-irb-readline-with-snow-leopard-homebrew-rvm">http://nruth.tumblr.com/post/858778765/fixing-irb-readline-with-snow-leopard-homebrew-rvm</a>

brew install readline
brew link readline (may be optional, install should do this already)
rvm --reconfigure --force -C --with-readline-dir=/usr/local install 1.8.7
rvm --reconfigure --force -C --with-readline-dir=/usr/local install ree
