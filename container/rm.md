##刪除容器
可以使用 `docker rm` 來刪除一個處於終止狀態的容器。
例如
```
$sudo docker rm  trusting_newton
trusting_newton
```
如果要刪除一個執行中的容器，可以加入 `-f` 參數。Docker 會傳送 `SIGKILL` 信號給容器。


##清理所有處於終止狀態的容器
用 `docker ps -a` 指令可以 檢視所有已經建立的包括終止狀態的容器，如果數量太多要一個個刪除可能會很麻煩，用 `docker rm $(docker ps -a -q)` 可以全部清理掉。

*注意：這個指令其實會試圖刪除所有的包括還在執行中的容器，不過就像上面提過的 `docker rm`  預設並不會刪除執行中的容器。