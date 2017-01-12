## 資料卷容器
如果你有一些持續更新的資料需要在容器之間共享，最好建立資料卷容器。

資料卷容器，其實就是一個正常的容器，專門用來提供資料卷供其它容器載入的。

首先，建立一個名為 dbdata 的資料卷容器：
```
$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
```
然後，在其它容器中使用 `--volumes-from` 來載入 dbdata 容器中的資料卷。
```
$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
```
可以使用超過一個的 `--volumes-from` 參數來指定從多個容器載入不同的資料卷。
也可以從其它已經載入了資料卷的容器來級聯載入資料卷。
```
$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
```
*注意：使用 `--volumes-from` 參數所載入資料卷的容器自己並不需要保持在執行狀態。

如果刪除了載入的容器（包括 dbdata、db1 和 db2），資料卷並不會被自動刪除。如果要刪除一個資料卷，必須在刪除最後一個還載入著它的容器時使用 `docker rm -v` 指令來指定同時刪除關聯的容器。
這可以讓使用者在容器之間升級和移動資料卷。具體的作業將在下一節中進行講解。
