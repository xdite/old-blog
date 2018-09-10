--- 
wordpress_id: 672
layout: post
title: HOWTO Build EC2 image
date: 2008-09-13 17:41:17 +08:00
wordpress_url: http://blog.xdite.net/?p=672
---
<a href="http://www.amazon.com/gp/browse.html?node=201590011">Amazon EC2</a> 是 Amazon 近年來推出的一個服務，主要是賣 computing power，拿來幹極盡邪惡的壞事相當適合。而如果對於網站有擴充需求或僅短期內有大量機器需求的網路公司也相當有幫助（當然架構要設計的比較適合扔上 ec2，或對如何自動 deploy 上 ec2 的工具以及流程比較熟稔）。

玩法通常是 build 自己的特定需求的幾個  image，如 web application / db server / memcache server blahblah 。static data 或 db 就設定塞到 <a href="http://www.amazon.com/gp/browse.html?node=16427261">s3</a> 和 <a href="http://blog.gslin.org/archives/2008/08/21/1632/">ebs</a> 去。如果短期有 event 需要因應大量訪客，狂開 ec2 通常就可以應付，而且可以省下蠻多機器和人力維護的費用（<a href="http://blog.gslin.org/archives/2008/09/13/1686/">因為如果也只有這麼一次，買機器相當不划算</a>）。

然而 Amazon EC2 並不是 dedicated server，也不保證資料完整性，它的玩法是出租 computing power 讓客戶跑自己的環境解決問題。因此如果要利用 EC2 弄 website，通常是 build 自己的 web production image、server image，打開打開打開 deploy,deploy,deploy 連接連接連接。

* 首先，如果你對玩 EC2 有興趣，個人強烈建議一定要裝的工具是由 Amazon 官方推出的 <a href="http://developer.amazonwebservices.com/connect/entry.jspa?externalID=609">Elasticfox</a> ( FF extension )，相當有用。

* 註冊 <a href="http://www.amazon.com/AWS-home-page-Money/b?ie=UTF8&node=3435361">amazon web serivces</a>，會拿到幾組東西。
 * Access Key ID
 * Secret Access Key
 * pk-XXXXX.pem
 * cert-XXXXXX.pem

<strong>- 管理你的 Amazon Instance</strong>

* 打開 Elasticfox，在 Credencials 填入 "account name"（自定）, "Access Key ID","Secret Access Key"。稍待片刻 (or refresh)之後，就能見到可用的 (public)Amazon Machine Image。

<a href="http://www.flickr.com/photos/xdite/2852181063/" title="Flickr 上 xdite 的 credencials"><img src="http://farm4.static.flickr.com/3015/2852181063_f0cd22a2c5.jpg" width="500" height="191" alt="credencials" /></a>

這裡的 AMI 都是包好的 clean image，可以開起來對 image 上下其手之後，再包成自己專用的 image（塞去 S3），以便下次開啟或大量複製。

<a href="http://www.flickr.com/photos/xdite/2853018302/" title="Flickr 上 xdite 的 ami"><img src="http://farm4.static.flickr.com/3024/2853018302_5a76066ebb.jpg" width="500" height="137" alt="ami" /></a>

任選一個按右鍵 launch 之後就可以把機器打開了。打開以後會像這樣子...

<a href="http://www.flickr.com/photos/xdite/2852188657/" title="Flickr 上 xdite 的 圖片 9"><img src="http://farm4.static.flickr.com/3093/2852188657_ebc9e96b50.jpg" width="500" height="75" alt="圖片 9" /></a>

值得注意的是，連過去管理是吃 key 不是吃密碼的，所以記得要去 "Key Pairs" 產生 Key。然後預設 Firefall 是通通開起來，所以要記得去 "Security Groups" 加規則讓 port 22 可以過。

ssh -i key_file root@aws-public_dns 就可以連過去可以管理了。

<strong>- 打包專屬的 AMI </strong>

裝完需要後的 package，就可以把這個已經通通弄好的環境包成 AMI。

* 首先需要將三樣東西 scp 去 /mnt 下
  * "Key Pairs" 產生的 Key
  * pk-XXXXX.pem
  * cert-XXXXXX.pem

- 產生 image 檔

<strong>ec2-bundle-vol -d /mnt/ -c cert-XXXXXX.pem -k pk-XXXXX.pem -u ACCOUNT_NUMBER(Your AWS Profile 裡面的那串數字） -r i386 -p TEST_AMI（你想取的名字）</strong>

blahblahblah 就會在 /mnt 生出一堆檔 ...

<strong>ec2-upload-bundle -b TEST_AMI -m TEST_AMI.manifest.xml -a Access_Key_ID -s Secret_Access_Key</strong>

開始扔上 S3。扔完之後會回吐 mainfest 的網址：

https://s3.amazonaws.com:443/TEST_AMI/TEST_AMI.manifest.xml

回 AMI 列表 Register ，把路徑<strong> TEST_AMI/TEST_AMI.manifest.xml</strong> submit 上去，過了幾秒就會出現你剛剛包的 image (private)。

<a href="http://www.flickr.com/photos/xdite/2853018302/" title="Flickr 上 xdite 的 ami"><img src="http://farm4.static.flickr.com/3024/2853018302_5a76066ebb.jpg" width="500" height="137" alt="ami" /></a>

之後就可以大量的把需要的環境叫起來。所以如果你只是測試玩玩的話，要記得關機器，因為每小時都要算錢，是直接從信用卡裡扣錢的 :Q （月結）

大概就是這樣子，操作步驟看起來蠻簡單的，不過沒有人帶的話，要花上很多功夫去看文章和猜設定 ..。所以最後要強烈感謝強者<a href="http://blog.gslin.org">壞人長輩</a>不嫌棄在下很笨，親自出手指點 <(_ _)>。

我自己 run 過一遍之後，把過程記在這裡。
