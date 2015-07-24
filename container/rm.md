##刪除容器
可以使用 `docker rm` 來刪除一個處於終止狀態的容器。
例如
```
$sudo docker rm  trusting_newton
trusting_newton
```
如果要刪除一個運行中的容器，可以添加 `-f` 參數。Docker 會發送 `SIGKILL` 信號給容器。


##清理所有處於終止狀態的容器
用 `docker ps -a` 命令可以查看所有已經創建的包括終止狀態的容器，如果數量太多要一個個刪除可能會很麻煩，用 `docker rm $(docker ps -a -q)` 可以全部清理掉。

*注意：這個命令其實會試圖刪除所有的包括還在運行中的容器，不過就像上面提過的 `docker rm` 預設並不會刪除運行中的容器。