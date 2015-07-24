## 什麼是 etcd

![](../_images/etcd_logo.png)

etcd 是 CoreOS 團隊於 2013 年 6 月發起的開源項目，它的目標是構建一個高可用的分佈式鍵值（key-value）數據庫，基於 Go 語言實現。我們知道，在分佈式系統中，各種服務的配置信息的管理分享，服務的發現是一個很基本同時也是很重要的問題。CoreOS 項目就希望基於 etcd 來解決這一問題。

etcd 目前在 [github.com/coreos/etcd](https://github.com/coreos/etcd) 進行維護，即將發佈 2.0.0 版本。

受到 [Apache ZooKeeper](http://zookeeper.apache.org/) 項目和 [doozer](https://github.com/ha/doozerd) 項目的啟發，etcd 在設計的時候重點考慮了下面四個要素：
* 簡單：支持 REST 風格的 HTTP+JSON API
* 安全：支持 HTTPS 方式的訪問
* 快速：支持並發 1k/s 的寫操作
* 可靠：支持分佈式結構，基於 Raft 的一致性算法

*註：Apache ZooKeeper 是一套知名的分佈式系統中進行同步和一致性管理的工具。*
*註：doozer 則是一個一致性分佈式數據庫。*
*註：Raft 是一套通過選舉主節點來實現分佈式系統一致性的算法，相比於大名鼎鼎的 Paxos 算法，它的過程更容易被人理解，由 Stanford 大學的 Diego Ongaro 和 John Ousterhout 提出。更多細節可以參考 [raftconsensus.github.io](http://raftconsensus.github.io)。*

一般情況下，用戶使用 etcd 可以在多個節點上啟動多個實例，並添加它們為一個集群。同一個集群中的 etcd 實例將會保持彼此信息的一致性。
