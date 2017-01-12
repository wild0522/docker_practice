# 基本架構

任何優秀的項目都離不開優秀的架構設計。本小節將介紹 Kubernetes 在架構方面的設計考慮。

## 基本考慮
如果讓我們自己從頭設計一套容器管理平台，有如下幾個方面是很容易想到的：
* 分散式架構，保證延伸性；
* 邏輯集中式的控制平面 + 物理分散式的執行平面；
* 一套資源調度系統，管理哪個容器該指派到哪個節點上；
* 一套對容器內服務進行抽像和 HA 的系統。

## 執行原理

下面這張圖完整展示了 Kubernetes 的執行原理。

![Kubernetes 架構](../_images/k8s_architecture.png)

可見，Kubernetes 首先是一套分散式系統，由多個節點組成，節點分為兩類：一類是屬於管理平面的主節點/控制節點（Master Node）；一類是屬於執行平面的工作節點（Worker Node）。

顯然，複雜的工作肯定都交給控制節點去做了，工作節點負責提供穩定的作業介面和能力抽像即可。

從這張圖上，我們沒有能發現 Kubernetes 中對於控制平面的分散式實作，但是由於資料後端自身就是一套分散式的資料庫（Etcd），因此可以很容易延伸到分散式實作。

## 控制平面
### 主節點服務
主節點上需要提供如下的管理服務：
* apiserver 是整個系統的對外介面，提供一套 RESTful 的 [Kubernetes API](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/api.md)，供用戶端和其它元件叫用；
* scheduler 負責對資源進行調度，指派某個 pod 到某個節點上。是 pluggable的，意味著很容易選擇其它實作方式；
* controller-manager 負責管理控制器，包括 endpoint-controller（重新整理服務和 pod 的關聯資訊）和 replication-controller（維護某個 pod 的複製為設定的數值）。

### Etcd
這裡 Etcd 即作為資料後端，又作為信息中間件。

透過 Etcd 來儲存所有的主節點上的狀態資訊，很容易實作主節點的分散式延伸。

元件可以自動的去偵測 Etcd 中的數值變化來獲得通知，並且獲得更新後的資料來執行相應的作業。

## 工作節點
* kubelet 是工作節點執行作業的 agent，負責具體的容器生命週期管理，根據從資料庫中取得的資訊來管理容器，並上報 pod 執行狀態等；
* kube-proxy 是一個簡單的網路存取代理，同時也是一個 Load Balancer。它負責將存取到某個服務的請求具體指派給工作節點上的 Pod（同一類標籤）。

![Proxy 代理對服務的請求](../_images/kube-proxy.png)
