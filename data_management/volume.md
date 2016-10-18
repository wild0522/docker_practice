## 資料卷
資料卷是一個可供一個或多個容器使用的特殊目錄，它繞過 UFS，可以提供很多有用的特性：
* 資料卷可以在容器之間共享和重用
* 對資料卷的修改會立馬生效
* 對資料卷的更新，不會影響鏡像
* 資料卷預設會一直存在，即使容器被刪除


*注意：資料卷的使用，類似於 Linux 下對目錄或文件進行 mount，鏡像中的被指定為掛載點的目錄中的文件會隱藏掉，能顯示看的是掛載的資料卷。


### 建立一個資料卷
在用 `docker run` 命令的時候，使用 `-v` 標記來建立一個資料卷並掛載到容器裡。在一次 run 中多次使用可以掛載多個資料卷。

下面建立一個名為 web 的容器，並加載一個資料捲到容器的 `/webapp` 目錄。
```
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```
*注意：也可以在 Dockerfile 中使用 `VOLUME` 來添加一個或者多個新的捲到由該鏡像建立的任意容器。

### 刪除資料卷
資料卷是被設計用來持久化資料的，它的生命週期獨立於容器，Docker不會在容器被刪除後自動刪除資料卷，並且也不存在垃圾回收這樣的機制來處理沒有任何容器引用的資料卷。如果需要在刪除容器的同時移除資料卷。可以在刪除容器的時候使用 `docker rm -v` 這個命令。無主的資料卷可能會佔據很多空間，要清理會很麻煩。Docker官方正在試圖解決這個問題，相關工作的進度可以查看這個[PR](https://github.com/docker/docker/pull/8484)

### 掛載一個主機目錄作為資料卷
使用 `-v` 標記也可以指定掛載一個本地主機的目錄到容器中去。
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```
上面的命令加載主機的 `/src/webapp` 目錄到容器的 `/opt/webapp`
目錄。這個功能在進行測試的時候十分方便，比如用戶可以放置一些程序到本地目錄中，來查看容器是否正常工作。本地目錄的路徑必須是絕對路徑，如果目錄不存在 Docker 會自動為你建立它。

*注意：Dockerfile 中不支持這種用法，這是因為 Dockerfile 是為了移植和分享用的。然而，不同操作系統的路徑格式不一樣，所以目前還不能支持。

Docker 掛載資料卷的預設權限是讀寫，用戶也可以通過 `:ro` 指定為只讀。
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro
training/webapp python app.py
```
加了 `:ro` 之後，就掛載為只讀了。

### 查看資料卷的具體訊息

在主機裡使用以下命令可以查看指定容器的訊息
```
$ docker inspect web
...
```

在輸出的內容中找到其中和資料卷相關的部分，可以看到所有的資料卷都是建立在主機的`/var/lib/docker/volumes/`下面的
```
"Volumes": {
    "/webapp": "/var/lib/docker/volumes/fac362...80535"
},
"VolumesRW": {
    "/webapp": true
}
...
```

### 掛載一個本地主機文件作為資料卷
`-v` 標記也可以從主機掛載單個文件到容器中
```
$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
```
這樣就可以記錄在容器輸入過的命令了。

*注意：如果直接掛載一個文件，很多文件編輯工具，包括 `vi` 或者 `sed --in-place`，可能會造成文件 inode 的改變，從 Docker 1.1
.0起，這會導致報錯誤訊息。所以最簡單的辦法就直接掛載文件的父目錄。
