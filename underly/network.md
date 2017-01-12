## Docker 網路實作

Docker 的網路實作其實就是利用了 Linux 上的網路命名空間和虛擬網路裝置（特別是 veth pair）。建議先熟悉瞭解這兩部分的基本概念再閱讀本章。

### 基本原理
首先，要實作網路通信，機器需要至少一個網路介面（物理介面或虛擬介面）來收發封包；此外，如果不同子網之間要進行通信，需要路由機制。

Docker 中的網路介面 預設都是虛擬的介面。虛擬介面的優勢之一是轉發效率較高。
Linux 透過在核心中進行資料複製來實作虛擬介面之間的資料轉發，傳送介面的傳送快取中的封包被直接複製到接收介面的接收快取中。對於本地系統和容器內系統看來就像是一個正常的以太網卡，只是它不需要真正同外部網路裝置通信，速度要快很多。

Docker 容器網路就利用了這項技術。它在本地主電腦和容器內分別建立一個虛擬介面，並讓它們彼此連通（這樣的一對介面叫做 `veth pair`）。

### 建立網路參數
Docker 建立一個容器的時候，會執行如下作業：
* 建立一對虛擬介面，分別放到本地主電腦和新容器中；
* 本地主電腦一端橋接到 預設的 docker0 或指定橋接器上，並具有一個唯一的名字，如 veth65f9；
* 容器一端放到新容器中，並修改名字作為 eth0，這個介面只在容器的命名空間可見；
* 從橋接器可用位址段中取得一個空閒位址指派給容器的 eth0，並設定 預設路由到橋接網路卡 veth65f9。

完成這些之後，容器就可以使用 eth0 虛擬網卡來連線其它容器和其它網路。

可以在 `docker run` 的時候透過 `--net` 參數來指定容器的網路設定，有4個可選值：
* `--net=bridge` 這個是預設值，連線到 預設的橋接器。
* `--net=host` 告訴 Docker 不要將容器網路放到隔離的命名空間中，即不要容器化容器內的網路。此時容器使用本地主電腦的網路，它擁有完全的本地主電腦介面存取許可。容器處理序可以跟主電腦其它 root 處理序一樣可以開啟低範圍的連接埠，可以存取本地網路服務比如 D-bus，還可以讓容器做一些影響整個主電腦系統的事情，比如重啟主電腦。因此使用這個選項的時候要非常小心。如果進一步的使用 `--privileged=true`，容器會被允許直接設定主電腦的網路堆堆疊。
* `--net=container:NAME_or_ID` 讓 Docker 將新增容器的處理序放到一個已存在容器的網路堆疊中，新容器處理序有自己的檔案系統、處理序清單和資源限制，但會和已存在的容器共享 IP 位址和連接埠等網路資源，兩者處理序可以直接透過 `lo` 環回介面通信。
* `--net=none` 讓 Docker 將新容器放到隔離的網路堆疊中，但是不進行網路設定。之後，使用者可以自己進行設定。

### 網路設定細節
使用者使用 `--net=none` 後，可以自行設定網路，讓容器達到跟平常一樣具有存取網路的權限。透過這個程序，可以瞭解 Docker 設定網路的細節。

首先，啟動一個 `/bin/bash` 容器，指定 `--net=none` 參數。
```
$ sudo docker run -i -t --rm --net=none base /bin/bash
root@63f36fc01b5f:/#
```
在本地主電腦搜尋容器的處理序 id，並為它建立網路命名空間。
```
$ sudo docker inspect -f '{{.State.Pid}}' 63f36fc01b5f
2778
$ pid=2778
$ sudo mkdir -p /var/run/netns
$ sudo ln -s /proc/$pid/ns/net /var/run/netns/$pid
```
檢查橋接網卡的 IP 和子網路遮罩資訊。
```
$ ip addr show docker0
21: docker0: ...
inet 172.17.42.1/16 scope global docker0
...
```
建立一對 「veth pair」 介面 A 和 B，繫結 A 到橋接器 `docker0`，並啟動它
```
$ sudo ip link add A type veth peer name B
$ sudo brctl addif docker0 A
$ sudo ip link set A up
```
將B放到容器的網路命名空間，命名為 eth0，啟動它並設定一個可用 IP（橋接網段）和 預設網關。
```
$ sudo ip link set B netns $pid
$ sudo ip netns exec $pid ip link set dev B name eth0
$ sudo ip netns exec $pid ip link set eth0 up
$ sudo ip netns exec $pid ip addr add 172.17.42.99/16 dev eth0
$ sudo ip netns exec $pid ip route add default via 172.17.42.1
```
以上，就是 Docker 設定網路的具體程序。

當容器結束後，Docker 會清理容器，容器內的 eth0 會隨網路命名空間一起被清除，A 介面也被自動從 `docker0` 卸載。

此外，使用者可以使用 `ip netns exec` 指令來在指定網路命名空間中進行設定，從而設定容器內的網路。
