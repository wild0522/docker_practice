## Mesos 常見框架

framework 是實際幹活的，可以理解為 mesos 上跑的 `應用`，需要註冊到 master 上。

### 長期執行的服務

#### [Aurora](http://aurora.incubator.apache.org/)
利用 mesos 調度安排的工作，保證工作一直在執行。

提供 REST 介面，用戶端和 webUI（8081 連接埠）

#### [Marathon](https://github.com/mesosphere/marathon)
一個 PaaS 平台。

保證工作一直在執行。如果停止了，會自動重啟一個新的工作。

支援工作為任意 bash 指令，以及容器。

提供 REST 介面，用戶端和 webUI（8080 連接埠）

#### [Singularity](https://github.com/HubSpot/Singularity)
一個 PaaS 平台。

調度器，執行長期的工作和一次性工作。

提供 REST 介面，用戶端和 webUI（7099、8080 連接埠），支援容器。

### 大資料處理
#### [Cray Chapel](https://github.com/nqn/mesos-chapel)
支援 Chapel 並行程式設計語言的執行框架。

#### [Dpark](https://github.com/douban/dpark)
Spark 的 Python 實作。

#### [Hadoop](https://github.com/mesos/hadoop)
經典的 map-reduce 模型的實作。

#### [Spark](http://spark.incubator.apache.org/)
跟 Hadoop 類似，但處理反覆類型工作會更好的使用 記憶體做中間狀態快取，速度要快一些。

#### [Storm](https://github.com/mesosphere/storm-mesos)
分散式流計算，可以即時處理資料串流。

### 批量調度
#### [Chronos](https://github.com/airbnb/chronos)
Cron 的分散式實作，負責工作調度。

#### [Jenkins](https://github.com/jenkinsci/mesos-plugin)
大名鼎鼎的 CI 引擎。使用 mesos-jenkins 外掛，可以將 jenkins 的工作被 mesos 來動態調度執行。

#### [ElasticSearch](https://github.com/mesosphere/elasticsearch-mesos)
功能十分強大的分散式資料搜尋引擎。

### 資料儲存
#### [Cassandra](https://github.com/mesosphere/cassandra-mesos)
高效能分散式資料庫。
