## Docker Hub
目前 Docker 官方維護了一個公共倉庫 [Docker Hub](https://hub.docker.com/)，其中已經包括了超過 15,000 的映像檔。大部分需求，都可以通過在 Docker Hub 中直接下載映像檔來實現。

### 登錄
可以通過執行 `docker login` 命令來輸入用戶名、密碼和郵箱來完成註冊和登錄。
註冊成功後，本地用戶目錄的 `.dockercfg` 中將保存用戶的認證信息。

### 基本操作
用戶無需登錄即可通過 `docker search` 命令來查找官方倉庫中的映像檔，並利用 `docker pull` 命令來將它下載到本地。

例如以 centos 為關鍵詞進行搜索：
```
$ sudo docker search centos
NAME                                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                                          The official build of CentOS.                   465       [OK]
tianon/centos                                   CentOS 5 and 6, created using rinse instea...   28
blalor/centos                                   Bare-bones base CentOS 6.5 image                6                    [OK]
saltstack/centos-6-minimal                                                                      6                    [OK]
tutum/centos-6.4                                DEPRECATED. Use tutum/centos:6.4 instead. ...   5                    [OK]
...
```
可以看到返回了很多包含關鍵字的映像檔，其中包括映像檔名字、描述、星級（表示該映像檔的受歡迎程度）、是否官方創建、是否自動創建。
官方的映像檔說明是官方項目組創建和維護的，automated 資源允許用戶驗證映像檔的來源和內容。

根據是否是官方提供，可將映像檔資源分為兩類。
一種是類似 centos 這樣的基礎映像檔，被稱為基礎或根映像檔。這些基礎映像檔是由 Docker 公司創建、驗證、支持、提供。這樣的映像檔往往使用單個單詞作為名字。
還有一種類型，比如 `tianon/centos` 映像檔，它是由 Docker 的用戶創建並維護的，往往帶有用戶名稱前綴。可以通過前綴 `user_name/` 來指定使用某個用戶提供的映像檔，比如 tianon 用戶。

另外，在查找的時候通過 `-s N` 參數可以指定僅顯示評價為 `N` 星以上的映像檔。

下載官方 centos 映像檔到本地。
```
$ sudo docker pull centos
Pulling repository centos
0b443ba03958: Download complete
539c0211cd76: Download complete
511136ea3c5a: Download complete
7064731afe90: Download complete
```
用戶也可以在登錄後通過 `docker push` 命令來將映像檔推送到 Docker Hub。

### 自動創建
自動創建（Automated Builds）功能對於需要經常升級映像檔內程序來說，十分方便。
有時候，用戶創建了映像檔，安裝了某個軟件，如果軟件發佈新版本則需要手動更新映像檔。。

而自動創建允許用戶通過 Docker Hub 指定跟蹤一個目標網站（目前支持 [GitHub](github.org) 或 [BitBucket](bitbucket.org)）上的項目，一旦項目發生新的提交，則自動執行創建。

要配置自動創建，包括如下的步驟：
* 創建並登錄 Docker Hub，以及目標網站；
* 在目標網站中連接帳戶到 Docker Hub；
* 在 Docker Hub 中 [配置一個自動創建](https://registry.hub.docker.com/builds/add/)；
* 選取一個目標網站中的項目（需要含 Dockerfile）和分支；
* 指定 Dockerfile 的位置，並提交創建。

之後，可以 在Docker Hub 的 [自動創建頁面](https://registry.hub.docker.com/builds/) 中跟蹤每次創建的狀態。
