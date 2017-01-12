## 簡介
Docker Swarm 是 Docker公司官方在 2014 年 12月初發佈的一套管理 Docker 叢集的工具。它將一群 Docker 宿主電腦變成一個單一的，虛擬的主電腦。

Swarm 使用標準的 Docker API 介面作為其前端存取入口，換言之，各種形式的 Docker 工具比如 Dokku，Compose，Krane，Deis，docker-py，Docker 本身等都可以很容易的與 Swarm 進行整合。

![Swarm 結構圖](../_images/swarm.png)

在使用 Swarm 管理docker 叢集時，會有一個 swarm manager 以及若干的 swarm node，swarm manager上執行 swarm daemon，使用者只需要跟 swarm manager 通信，然後 swarm manager 再根據discovery service的資訊選擇一個swarm node 來執行container。

值得注意的是 swarm daemon 只是一個工作調度器(scheduler)和路由器(router),它本身不執行容器，它只接受 Docker client 傳送過來的請求，調度合適的 swarm node 來執行 container。這意味著，即使 swarm daemon 由於某些原因掛掉了，已經執行起來的容器也不會有任何影響。


有以下兩點需要注意：

* 叢集中的每台節點上面的 Docker 的版本都不能小於1.4
* 為了讓 swarm manager 能夠跟每台 swarm node 進行通信，叢集中的每台節點的 Docker daemon 都必須監聽同一個網路介面。
