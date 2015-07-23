## Mesos 常見框架

framework 是實際幹活的，可以理解為 mesos 上跑的 `應用`，需要註冊到 master 上。

### 長期運行的服務

#### [Aurora](http://aurora.incubator.apache.org/)
利用 mesos 調度安排的任務，保證任務一直在運行。

提供 REST 接口，客戶端和 webUI（8081 端口）

#### [Marathon](https://github.com/mesosphere/marathon)
一個 PaaS 平台。

保證任務一直在運行。如果停止了，會自動重啟一個新的任務。

支持任務為任意 bash 命令，以及容器。

提供 REST 接口，客戶端和 webUI（8080 端口）

#### [Singularity](https://github.com/HubSpot/Singularity)
一個 PaaS 平台。

調度器，運行長期的任務和一次性任務。

提供 REST 接口，客戶端和 webUI（7099、8080 端口），支持容器。

### 大數據處理
#### [Cray Chapel](https://github.com/nqn/mesos-chapel)
支持 Chapel 並行編程語言的運行框架。

#### [Dpark](https://github.com/douban/dpark)
Spark 的 Python 實現。

#### [Hadoop](https://github.com/mesos/hadoop)
經典的 map-reduce 模型的實現。

#### [Spark](http://spark.incubator.apache.org/)
跟 Hadoop 類似，但處理迭代類型任務會更好的使用內存做中間狀態緩存，速度要快一些。

#### [Storm](https://github.com/mesosphere/storm-mesos)
分佈式流計算，可以實時處理數據流。

### 批量調度
#### [Chronos](https://github.com/airbnb/chronos)
Cron 的分佈式實現，負責任務調度。

#### [Jenkins](https://github.com/jenkinsci/mesos-plugin)
大名鼎鼎的 CI 引擎。使用 mesos-jenkins 插件，可以將 jenkins 的任務被 mesos 來動態調度執行。

#### [ElasticSearch](https://github.com/mesosphere/elasticsearch-mesos)
功能十分強大的分佈式數據搜索引擎。

### 數據存儲
#### [Cassandra](https://github.com/mesosphere/cassandra-mesos)
高性能分佈式數據庫。
