## 簡介
Docker Swarm 是 Docker公司官方在 2014 年 12月初發佈的一套管理 Docker 集群的工具。它將一群 Docker 宿主機變成一個單一的，虛擬的主機。

Swarm 使用標準的 Docker API 接口作為其前端訪問入口，換言之，各種形式的 Docker 工具比如 Dokku，Compose，Krane，Deis，docker-py，Docker 本身等都可以很容易的與 Swarm 進行集成。

![Swarm 結構圖](../images/swarm.png)

在使用 Swarm 管理docker 集群時，會有一個 swarm manager 以及若干的 swarm node，swarm manager上運行 swarm daemon，用戶只需要跟 swarm manager 通信，然後 swarm manager 再根據discovery service的信息選擇一個swarm node 來運行container。

值得注意的是 swarm daemon 只是一個任務調度器(scheduler)和路由器(router),它本身不運行容器，它只接受 Docker client 發送過來的請求，調度合適的 swarm node 來運行 container。這意味著，即使 swarm daemon 由於某些原因掛掉了，已經運行起來的容器也不會有任何影響。


有以下兩點需要注意：

* 集群中的每台節點上面的 Docker 的版本都不能小於1.4
* 為了讓 swarm manager 能夠跟每台 swarm node 進行通信，集群中的每台節點的 Docker daemon 都必須監聽同一個網路接口。
