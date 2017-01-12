## 利用資料卷容器來備份、恢復、遷移資料卷
可以利用資料卷對其中的資料進行進行備份、恢復和遷移。

### 備份
首先使用 `--volumes-from` 標記來建立一個載入 dbdata 容器卷的容器，並從主電腦載入目前目錄到容器的 /backup 目錄。指令如下：
```
$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```
容器啟動後，使用了 `tar` 指令來將 dbdata 卷備份為容器中 /backup/backup.tar 檔案，也就是主電腦目前目錄下的名為 `backup.tar` 的檔案。


### 恢復
如果要恢復資料到一個容器，首先建立一個帶有空資料卷的容器 dbdata2。
```
$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
```
然後建立另一個容器，載入 dbdata2 容器卷中的資料卷，並使用 `untar` 解壓備份檔到載入的容器卷中。
```
$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
/backup/backup.tar
```
為了 檢視/驗證恢復的資料，可以再啟動一個容器載入同樣的容器捲來 檢視
```
$ sudo docker run --volumes-from dbdata2 busybox /bin/ls /dbdata
```