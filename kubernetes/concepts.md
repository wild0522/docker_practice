# 基本概念

![](../_images/kubernetes_design.jpg)

* 節點（Node）：一個節點是一個執行 Kubernetes 中的主電腦。
* 容器組（Pod）：一個 Pod 對應於由若干容器組成的一個容器組，同個組內的容器共享一個儲存卷(volume)。
* 容器組生命週期（pos-states）：包含所有容器狀態集合，包括容器組狀態類型，容器組生命週期，事件，重啟原則，以及replication controllers。
* Replication Controllers（replication-controllers）：主要負責指定數量的pod在同一時間一起執行。
* 服務（services）：一個Kubernetes服務是容器組邏輯的進階抽像，同時也對外提供存取容器組的原則。
* 卷（volumes）：一個卷就是一個目錄，容器對其有存取許可。
* 標籤（labels）：標籤是用來連線一組對象的，比如容器組。標籤可以被用來組織和選擇子物件。
* 介面權限（accessing_the_api）：連接埠，ip位址和代理的防火牆規則。
* web 界面（ux）：使用者可以透過 web 界面作業Kubernetes。
* 指令行指令列作業（cli）：`kubecfg`指令。


## 節點
在 Kubernetes 中，節點是實際工作的點，以前叫做 Minion。節點可以是虛擬機或是物理機器，依賴於一個叢集環境。每個節點都有一些必要的服務以執行容器組，並且它們都可以透過主節點來管理。必要服務包括 Docker，kubelet 和代理服務。

### 容器狀態

容器狀態用來描述節點的目前狀態。現在，其中包含三個資訊：

#### 主電腦IP

主電腦IP需要雲平台來查詢，Kubernetes把它作為狀態的一部分來儲存。如果Kubernetes沒有執行在雲平台上，節點ID就是必需的。IP位址可以變化，並且可以包含多種類型的IP位址，如公共IP，私有IP，動態IP，ipv6等等。

#### 節點週期

通常來說節點有 `Pending`，`Running`，`Terminated`三個週期，如果Kubernetes發現了一個節點並且其可用，那麼Kubernetes就把它標記為 `Pending`。然後在某個時刻，Kubernetes將會標記其為 `Running`。節點的結束週期稱為 `Terminated`。一個已經terminated的節點不會接受和調度任何請求，並且已經在其上執行的容器組也會刪除。

#### 節點狀態

節點的狀態主要是用來描述處於 `Running`的節點。目前可用的有 `NodeReachable` 和 `NodeReady` 。以後可能會增加其它狀態。`NodeReachable` 表示叢集可達。`NodeReady`表示kubelet傳回 StatusOk並且HTTP狀態檢查健康。

### 節點管理

節點並非Kubernetes建立，而是由雲平台建立，或是就是物理機器、虛擬機。在Kubernetes中，節點僅僅是一條記錄，節點建立之後，Kubernetes會檢查其是否可用。在Kubernetes中，節點用如下結構儲存：

```
{
  "id": "10.1.2.3",
  "kind": "Minion",
  "apiVersion": "v1beta1",
  "resources": {
    "capacity": {
      "cpu": 1000,
      "memory": 1073741824
    },
  },
  "labels": {
    "name": "my-first-k8s-node",
  },
}
```

Kubernetes校驗節點可用依賴於id。在目前的版本中，有兩個介面可以用來管理節點：節點控制和Kube管理。

### 節點控制

在Kubernetes主節點中，節點控制器是用來管理節點的元件。主要包含：
* 叢集範圍內節點同步
* 單節點生命週期管理

節點控制有一個同步輪尋，主要監聽所有雲平台的虛擬實例，會根據節點狀態建立和刪除。可以透過 `--node_sync_period`標誌來控制該輪尋。如果一個實例已經建立，節點控制將會為其建立一個結構。同樣的，如果一個節點被刪除，節點控制也會刪除該結構。在Kubernetes啟動時可用透過 `--machines`標記來顯示指定節點。同樣可以使用 `kubectl`來一條一條的加入節點，兩者是相同的。透過設定 `--sync_nodes=false`標記來禁止叢集之間的節點同步，你也可以使用api/kubectl 指令行指令列來增刪節點。

## 容器組

在Kubernetes中，使用的最小單位是容器組，容器組是建立，調度，管理的最小單位。
一個容器組使用相同的Dokcer容器並共享卷（載入點）。一個容器組是一個特定運用的打包集合，包含一個或多個容器。

和執行的容器類似，一個容器組被認為只有很短的執行週期。容器組被調度到一組節點執行，知道容器的生命週期結束或是其被刪除。如果節點死掉，執行在其上的容器組將會被刪除而不是重新調度。（也許在將來的版本中會加入容器組的移動）。

### 容器組設計的初衷

### 資源共享和通信

容器組主要是為了資料共享和它們之間的通信。

在一個容器組中，容器都使用相同的網路位址和連接埠，可以透過本地網路來相互通信。每個容器組都有獨立的ip，可用透過網路來和其它物理主電腦或是容器通信。

容器組有一組儲存卷（載入點），主要是為了讓容器在重啟之後可以不丟失資料。

### 容器組管理

容器組是一個運用管理和部署的高層次抽像，同時也是一組容器的介面。容器組是部署、水平放縮的最小單位。

### 容器組的使用

容器組可以透過組合來建構複雜的運用，其本來的意義包含：

* 內容管理，檔案和資料載入以及本地快取管理等。
* 日誌和檢查點備份，壓縮，快照等。
* 監聽資料變化，追蹤日誌，日誌和監控代理，信息發佈等。
* 代理，橋接器
* 控制器，管理，設定以及更新

### 替代專案

為什麼不在一個單一的容器裡執行多個程式？

* 1.透明化。為了使容器組中的容器保持一致的基礎設施和服務，比如處理序管理和資源監控。這樣設計是為了使用者的便利性。
* 2.解偶軟體之間的依賴。每個容器都可能重新建構和發佈，Kubernetes必須支援熱發佈和熱更新（將來）。
* 3.方便使用。使用者不必執行獨立的程式管理，也不用擔心每個運用程式的結束狀態。
* 4.高效。考慮到基礎設施有更多的職責，容器必須要輕量化。

### 容器組的生命狀態

包括若干狀態值：pending、running、succeeded、failed。

#### pending

容器組已經被節點接受，但有一個或多個容器還沒有執行起來。這將包含某些節點正在下載映像檔的時間，這種情形會依賴於網路情況。

#### running

容器組已經被調度到節點，並且所有的容器都已經啟動。至少有一個容器處於執行狀態（或是處於重啟狀態）。

#### succeeded

所有的容器都正常結束。

#### failed

容器組中所有容器都意外插斷了。

### 容器組生命週期

通常來說，如果容器組被建立了就不會自動終結，除非被某種行為出發，而觸發此種情況可能是人為，或是複製控制器所為。唯一例外的是容器組由 succeeded狀態成功結束，或是在一定時間內重試多次依然失敗。

如果某個節點死掉或是不能連線，那麼節點控制器將會標記其上的容器組的狀態為 `failed`。

舉例如下。

* 容器組狀態 `running`，有 1 容器，容器正常結束
     * 記錄完成事件
     * 如果重啟原則為：
        * 始終：重啟容器，容器組保持 `running`
        * 失敗時：容器組變為 `succeeded`
        * 永遠不：容器組變為 `succeeded`
* 容器組狀態 `running`，有1容器，容器異常結束
     * 記錄失敗事件
     * 如果重啟原則為：
        * 始終：重啟容器，容器組保持 `running`
        * 失敗時：重啟容器，容器組保持 `running`
        * 永遠不：容器組變為 `failed`
* 容器組狀態 `running`，有2容器，有1容器異常結束
     * 記錄失敗事件
     * 如果重啟原則為：
        * 始終：重啟容器，容器組保持 `running`
        * 失敗時：重啟容器，容器組保持 `running`
        * 永遠不：容器組保持 `running`
    * 當有2容器結束
        * 記錄失敗事件
        * 如果重啟原則為：
            * 始終：重啟容器，容器組保持 `running`
            * 失敗時：重啟容器，容器組保持 `running`
            * 永遠不：容器組變為 `failed`
* 容器組狀態 `running`，容器記憶體不足
    * 標記容器錯誤插斷
    * 記錄記憶體不足事件
    * 如果重啟原則為：
        * 始終：重啟容器，容器組保持 `running`
        * 失敗時：重啟容器，容器組保持 `running`
        * 永遠不：記錄錯誤事件，容器組變為 `failed`
* 容器組狀態 `running`，一塊磁碟死掉
    * 殺死所有容器
    * 記錄事件
    * 容器組變為 `failed`
    * 如果容器組執行在一個控制器下，容器組將會在其它地方重新建立
* 容器組狀態 `running`，對應的節點段溢出
    * 節點控制器等到逾時
    * 節點控制器標記容器組 `failed`
    * 如果容器組執行在一個控制器下，容器組將會在其它地方重新建立

## Replication Controllers
## 服務
## 卷
## 標籤
## 介面權限
## web界面
## 指令行指令列作業
