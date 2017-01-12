## 資料卷
資料卷是一個可供一個或多個容器使用的特殊目錄，它繞過 UFS，可以提供很多有用的內容屬性：
* 資料卷可以在容器之間共享和重用
* 對資料卷的修改會立馬生效
* 對資料卷的更新，不會影響映像檔
* 資料卷 預設會一直存在，即使容器被刪除


*注意：資料卷的使用，類似於 Linux 下對目錄或檔案進行 mount，映像檔中的被指定為載入點的目錄中的檔案會隱藏掉，能顯示看的是載入的資料卷。


### 建立一個資料卷
在用 `docker run` 指令的時候，使用 `-v` 標記來建立一個資料卷並載入到容器裡。在一次 run 中多次使用可以載入多個資料卷。

下面建立一個名為 web 的容器，並載入一個資料捲到容器的 `/webapp` 目錄。
```
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```
*注意：也可以在 Dockerfile 中使用 `VOLUME` 來加入一個或是多個新的捲到由該映像檔建立的任意容器。

### 刪除資料卷
資料卷是被設計用來持久化資料的，它的生命週期獨立於容器，Docker不會在容器被刪除後自動刪除資料卷，並且也不存在垃圾回收這樣的機制來處理沒有任何容器參考的資料卷。如果需要在刪除容器的同時移除資料卷。可以在刪除容器的時候使用 `docker rm -v` 這個指令。無主的資料卷可能會佔據很多空間，要清理會很麻煩。Docker官方正在試圖解決這個問題，相關工作的進度可以 檢視這個[PR](https://github.com/docker/docker/pull/8484)。

### 載入一個主電腦目錄作為資料卷
使用 `-v` 標記也可以指定載入一個本地主電腦的目錄到容器中去。
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```
上面的指令載入主電腦的 `/src/webapp` 目錄到容器的 `/opt/webapp`
目錄。這個功能在進行測試的時候十分方便，比如使用者可以放置一些程式到本地目錄中，來 檢視容器是否正常工作。本地目錄的路徑必須是絕對路徑，如果目錄不存在 Docker 會自動為你建立它。

*注意：Dockerfile 中不支援這種使用方式，這是因為 Dockerfile 是為了移植和分享用的。然而，不同作業系統的路徑格式不一樣，所以目前還不能支援。

Docker 載入資料卷的 預設權限是讀寫，使用者也可以透過 `:ro` 指定為唯讀。
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro
training/webapp python app.py
```
加了 `:ro` 之後，就載入為唯讀了。

###  檢視資料卷的具體資訊

在主電腦裡使用以下指令可以 檢視指定容器的資訊
```
$ docker inspect web
...
```

在輸出的內容中找到其中和資料卷相關的部分，可以看到所有的資料卷都是建立在主電腦的`/var/lib/docker/volumes/`下面的
```
"Volumes": {
    "/webapp": "/var/lib/docker/volumes/fac362...80535"
},
"VolumesRW": {
    "/webapp": true
}
...
```

### 載入一個本地主電腦檔案作為資料卷
`-v` 標記也可以從主電腦載入單個檔案到容器中
```
$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
```
這樣就可以記錄在容器輸入過的指令了。

*注意：如果直接載入一個檔案，很多檔案編輯工具，包括 `vi` 或是 `sed --in-place`，可能會造成檔案 inode 的改變，從 Docker 1.1
.0起，這會導致報錯誤資訊。所以最簡單的辦法就直接載入檔案的父目錄。
