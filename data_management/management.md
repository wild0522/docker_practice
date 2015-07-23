## 利用數據卷容器來備份、恢復、遷移數據卷
可以利用數據卷對其中的數據進行進行備份、恢復和遷移。

### 備份
首先使用 `--volumes-from` 標記來創建一個加載 dbdata 容器卷的容器，並從主機掛載當前目錄到容器的 /backup 目錄。命令如下：
```
$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```
容器啟動後，使用了 `tar` 命令來將 dbdata 卷備份為容器中 /backup/backup.tar 文件，也就是主機當前目錄下的名為 `backup.tar` 的文件。


### 恢復
如果要恢復數據到一個容器，首先創建一個帶有空數據卷的容器 dbdata2。
```
$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
```
然後創建另一個容器，掛載 dbdata2 容器卷中的數據卷，並使用 `untar` 解壓備份文件到掛載的容器卷中。
```
$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
/backup/backup.tar
```
為了查看/驗證恢復的數據，可以再啟動一個容器掛載同樣的容器捲來查看
```
$ sudo docker run --volumes-from dbdata2 busybox /bin/ls /dbdata
```