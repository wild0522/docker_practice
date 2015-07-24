Docker —— 從入門到實踐(Benson test) 
===============

v0.5.1

[Docker](http://www.docker.com) 是個偉大的項目，它徹底釋放了虛擬化的威力，極大降低了雲計算資源供應的成本，同時讓應用的部署、測試和分發都變得前所未有的高效和輕鬆！

本書既適用於具備基礎 Linux 知識的 Docker 初學者，也希望可供理解原理和實現的高級用戶參考。同時，書中給出的實踐案例，可供在進行實際部署時借鑒。前六章為基礎內容，供用戶理解 Docker 的基本概念和操作；7 ~ 9 章介紹一些高級操作；第 10 章給出典型的應用場景和實踐案例；11 ~ 13 章介紹關於 Docker 實現的相關細節技術。後續章節則分別介紹一些相關的熱門開源項目。

在線閱讀：[GitBook](https://www.gitbook.io/book/yeasy/docker_practice) 或 [DockerPool](http://dockerpool.com/static/books/docker_practice/index.html)。

歡迎關注 DockerPool 社區微博 [@dockerpool](http://weibo.com/u/5345404432)，或加入 DockerPool QQ 群（419042067），分享 Docker 資源，交流 Docker 技術。

![Docker 技術入門與實戰](docker_primer.png)

《[Docker 技術入門與實戰](http://item.jd.com/11598400.html)》一書已經正式出版，包含大量第一手實戰案例和更為深入的技術剖析，歡迎大家閱讀使用。

* [China-Pub](http://product.china-pub.com/3770833)
* [京東圖書](http://item.jd.com/11598400.html)
* [當當圖書](http://product.dangdang.com/23620853.html)
* [亞馬遜圖書](http://www.amazon.cn/%E5%9B%BE%E4%B9%A6/dp/B00R5MYI7C/ref=lh_ni_t?ie=UTF8&psc=1&smid=A1AJ19PSB66TGU)

## 主要版本歷史
* 0.6: 2015-07-?
    * 補充 Machine 項目
* 0.5: 2015-06-29
    * 添加 Compose 項目
    * 添加 Machine 項目
    * 添加 Swarm 項目
    * 完善 Kubernetes 項目內容
    * 添加 Mesos 項目內容
* 0.4: 2015-05-08
    * 添加 Etcd 項目
    * 添加 Fig 項目
    * 添加 CoreOS 項目
    * 添加 Kubernetes 項目
* 0.3: 2014-11-25
    * 完成倉庫章節；
    * 重寫安全章節；
    * 修正底層實現章節的架構、名字空間、控制組、文件系統、容器格式等內容；
    * 添加對常見倉庫和鏡像的介紹；
    * 添加 Dockerfile 的介紹；
    * 重新校訂中英文混排格式。
    * 修訂文字表達。
    * 發佈繁體版本分支：zh-Hant。
* 0.2: 2014-09-18
    * 對照官方文檔重寫介紹、基本概念、安裝、鏡像、容器、倉庫、數據管理、網絡等章節；
    * 添加底層實現章節；
    * 添加命令查詢和資源鏈接章節；
    * 其它修正。
* 0.1: 2014-09-05
    * 添加基本內容;
    * 修正錯別字和表達不通順的地方。


Docker 自身仍在快速發展中，生態環境也在蓬勃成長。源碼開源托管在 Github 上，歡迎參與維護：[https://github.com/yeasy/docker_practice](https://github.com/yeasy/docker_practice)。貢獻者 [名單](https://github.com/yeasy/docker_practice/graphs/contributors)。

## 參加步驟
* 在 GitHub 上 `fork` 到自己的倉庫，如 `docker_user/docker_practice`，然後 `clone` 到本地，並設置用戶信息。
```
$ git clone git@github.com:docker_user/docker_practice.git
$ cd docker_practice
$ git config user.name "yourname"
$ git config user.email "your email"
```
* 修改代碼後提交，並推送到自己的倉庫。
```
$ #do some change on the content
$ git commit -am "Fix issue #1: change helo to hello"
$ git push
```
* 在 GitHub 網站上提交 pull request。
* 定期使用項目倉庫內容更新自己倉庫內容。
```
$ git remote add upstream https://github.com/yeasy/docker_practice
$ git fetch upstream
$ git checkout master
$ git rebase upstream/master
$ git push -f origin master
```
