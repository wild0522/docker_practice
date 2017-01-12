## 什麼是 etcd

![](../_images/etcd_logo.png)

etcd 是 CoreOS 團隊於 2013 年 6 月發起的開源項目，它的目標是建構一個高可用的分散式鍵值（key-value）資料庫，基於 Go 語言實作。我們知道，在分散式系統中，各種服務的設定資訊的管理分享，服務的發現是一個很基本同時也是很重要的問題。CoreOS 項目就希望基於 etcd 來解決這一問題。

etcd 目前在 [github.com/coreos/etcd](https://github.com/coreos/etcd) 進行維護，即將發佈 2.0.0 版本。

受到 [Apache ZooKeeper](http://zookeeper.apache.org/) 項目和 [doozer](https://github.com/ha/doozerd) 項目的啟發，etcd 在設計的時候重點考慮了下面四個要素：
* 簡單：支援 REST 風格的 HTTP+JSON API
* 安全：支援 HTTPS 方式的存取
* 快速：支援並發 1k/s 的寫作業
* 可靠：支援分散式結構，基於 Raft 的一致性算法

*註：Apache ZooKeeper 是一套知名的分散式系統中進行同步和一致性管理的工具。*
*註：doozer 則是一個一致性分散式資料庫。*
*註：Raft 是一套透過選舉主節點來實作分散式系統一致性的算法，相比於大名鼎鼎的 Paxos 算法，它的程序更容易被人理解，由 Stanford 大學的 Diego Ongaro 和 John Ousterhout 提出。更多細節可以參考 [raftconsensus.github.io](http://raftconsensus.github.io)。*

一般情況下，使用者使用 etcd 可以在多個節點上啟動多個實例，並加入它們為一個叢集。同一個叢集中的 etcd 實例將會保持彼此資訊的一致性。
