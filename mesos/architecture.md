## Mesos 基本原理與架構

首先，Mesos 自身只是一個資源調度框架，並非一整套完整的應用管理平台，本身是不能幹活的。但是它可以比較容易的跟各種應用管理或是中間件平台整合，一起工作，提高資源使用效率。

### 架構
![mesos-arch](../_images/mesos-architecture.png)
master-slave 架構，master 使用 zookeeper 來做 HA。

master 單獨執行在管理節點上，slave 執行在各個計算工作節點上。

各種具體工作的管理平台，即 framework 跟 master 互動，來申請資源。


### 基本單元

#### master
負責整體的資源調度和邏輯控制。

#### slave
負責匯報本節點上的資源給 master，並負責隔離資源來執行具體的工作。

隔離機制當然就是各種容器機制了。

#### framework
framework 是實際幹活的，包括兩個主要元件：

* scheduler：註冊到主節點，等待指派資源；
* executor：在 slave 節點上執行本framework 的工作。

framework 分兩種：一種是對資源需求可以 scale up 或是 down 的（Hadoop、Spark）；一種是對資源需求大小是固定的（MPI）。

### 調度
對於一個資源調度框架來說，最核心的就是調度機制，怎麼能快速高效的完成對某個 framework 資源的指派（最好是能猜到它的實際需求）。

#### 兩層調度算法：
master 先調度一大塊資源給某個 framework，framework 自己再實作內部的細粒度調度。

調度機制支援外掛。 預設是 DRF。

#### 基本調度程序
調度透過 offer 方式互動：

* master 提供一個 offer（一組資源） 給 framework；
* framework 可以決定要不要，如果接受的話，傳回一個描述，說明自己希望如何使用和指派這些資源（可以說明只希望使用部分資源，則多出來的會被 master 收回）；
* master 則根據 framework 的指派情況傳送給 slave，以使用 framework 的 executor 來按照指派的資源原則執行工作。

#### 篩選器
framework 可以透過篩選器機制告訴 master 它的資源偏好，比如希望指派過來的 offer 有哪個資源，或是至少有多少資源。

主要是為了加速資源指派的互動程序。

#### 回收機制
master 可以透過回收計算節點上的工作來動態調整長期工作和短期工作的分佈。


### HA

#### master
master 節點存在單點失效問題，所以肯定要上 HA，目前主要是使用 zookpeer 來熱備份。

同時 master 節點可以透過 slave 和 framework 發來的信息重建內部狀態（具體能有多快呢？這裡不使用資料庫可能是避免引入複雜度。）。

#### framework 通知
framework 中相關的失效，master 將發給它的 scheduler 來通知。
