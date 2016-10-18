## 資料卷容器
如果你有一些持續更新的資料需要在容器之間共享，最好創建資料卷容器。

資料卷容器，其實就是一個正常的容器，專門用來提供資料卷供其它容器掛載的。

首先，創建一個名為 dbdata 的資料卷容器：
```
$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
```
然後，在其他容器中使用 `--volumes-from` 來掛載 dbdata 容器中的資料卷。
```
$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
```
可以使用超過一個的 `--volumes-from` 參數來指定從多個容器掛載不同的資料卷。
也可以從其他已經掛載了資料卷的容器來級聯掛載資料卷。
```
$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
```
*注意：使用 `--volumes-from` 參數所掛載資料卷的容器自己並不需要保持在運行狀態。

如果刪除了掛載的容器（包括 dbdata、db1 和 db2），資料卷並不會被自動刪除。如果要刪除一個資料卷，必須在刪除最後一個還掛載著它的容器時使用 `docker rm -v` 命令來指定同時刪除關聯的容器。
這可以讓用戶在容器之間升級和移動資料卷。具體的操作將在下一節中進行講解。
