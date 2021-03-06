# 快速上手

目前，Kubenetes 支援在多種環境下的安裝，包括本地主電腦（Fedora）、雲服務（Google GAE、AWS 等）。然而最快速體驗 Kubernetes 的方式顯然是本地透過 Docker 的方式來啟動相關處理序。

下圖展示了在單節點使用 Docker 快速部署一套 Kubernetes 的拓撲。

![在 Docker 中啟動 Kubernetes](../_images/k8s-singlenode-docker.png)

Kubernetes 依賴 Etcd 服務來維護所有主節點的狀態。

## 啟動 Etcd 服務。
```bash
docker run --net=host -d gcr.io/google_containers/etcd:2.0.9 /usr/local/bin/etcd --addr=127.0.0.1:4001 --bind-addr=0.0.0.0:4001 --data-dir=/var/etcd/data
```

## 啟動主節點
啟動 kubelet。
```bash
docker run --net=host -d -v /var/run/docker.sock:/var/run/docker.sock  gcr.io/google_containers/hyperkube:v0.17.0 /hyperkube kubelet --api_servers=http://localhost:8080 --v=2 --address=0.0.0.0 --enable_server --hostname_override=127.0.0.1 --config=/etc/kubernetes/manifests
```

## 啟動服務代理
```bash
docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v0.17.0 /hyperkube proxy --master=http://127.0.0.1:8080 --v=2
```

## 測試狀態
在本地存取 8080 連接埠，應該取得到類似如下的結果：
```bash
$ curl 127.0.0.1:8080
{
  "paths": [
    "/api",
    "/api/v1beta1",
    "/api/v1beta2",
    "/api/v1beta3",
    "/healthz",
    "/healthz/ping",
    "/logs/",
    "/metrics",
    "/static/",
    "/swagger-ui/",
    "/swaggerapi/",
    "/validate",
    "/version"
  ]
}
```

##  檢視服務
所有服務啟動後過一會， 檢視本地實際執行的 Docker 容器，應該有如下幾個。
```bash
CONTAINER ID        IMAGE                                        COMMAND                CREATED             STATUS              PORTS               NAMES
ee054db2516c        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube schedule   2 days ago          Up 1 days                               k8s_scheduler.509f29c9_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_e97037f5
3b0f28de07a2        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube apiserve   2 days ago          Up 1 days                               k8s_apiserver.245e44fa_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_6ab5c23d
2eaa44ecdd8e        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube controll   2 days ago          Up 1 days                               k8s_controller-manager.33f83d43_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_1a60106f
30aa7163cbef        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube proxy --   2 days ago          Up 1 days                               jolly_davinci
a2f282976d91        gcr.io/google_containers/pause:0.8.0         "/pause"               2 days ago          Up 2 days                               k8s_POD.e4cc795_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_e8085b1f
c060c52acc36        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube kubelet    2 days ago          Up 1 days                               serene_nobel
cc3cd263c581        gcr.io/google_containers/etcd:2.0.9          "/usr/local/bin/etcd   2 days ago          Up 1 days                               happy_turing
```

這些服務大概分為三類：主節點服務、工作節點服務和其它服務。

### 主節點服務
* apiserver 是整個系統的對外介面，提供 RESTful 方式供用戶端和其它元件叫用；
* scheduler 負責對資源進行調度，指派某個 pod 到某個節點上；
* controller-manager 負責管理控制器，包括 endpoint-controller（重新整理服務和 pod 的關聯資訊）和 replication-controller（維護某個 pod 的複製為設定的數值）。

### 工作節點服務
* kubelet 是工作節點執行作業的 agent，負責具體的容器生命週期管理，根據從資料庫中取得的資訊來管理容器，並上報 pod 執行狀態等；
* proxy 為 pod 上的服務提供存取的代理。

### 其它服務
* etcd 是所有狀態的儲存資料庫；
* `gcr.io/google_containers/pause:0.8.0` 是 Kubernetes 啟動後自動 pull 下來的測試映像檔。
