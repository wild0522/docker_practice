## 安裝

etcd 基於 Go 語言實作，因此，使用者可以從 [項目首頁](https://github.com/coreos/etcd) 下載原始碼自行編譯，也可以下載編譯好的二進位檔案，甚至直接使用製作好的 Docker 映像檔檔案來體驗。

### 二進位檔案方式下載

編譯好的二進位檔案都在 [github.com/coreos/etcd/releases](https://github.com/coreos/etcd/releases/) 頁面，使用者可以選擇需要的版本，或透過下載工具下載。

例如，下面的指令使用 curl 工具下載壓縮包，並解壓。

```
curl -L  https://github.com/coreos/etcd/releases/download/v2.0.0-rc.1/etcd-v2.0.0-rc.1-linux-amd64.tar.gz -o etcd-v2.0.0-rc.1-linux-amd64.tar.gz
tar xzvf etcd-v2.0.0-rc.1-linux-amd64.tar.gz
cd etcd-v2.0.0-rc.1-linux-amd64
```

解壓後，可以看到檔案包括
```
$ ls
etcd  etcdctl  etcd-migrate  README-etcdctl.md  README.md
```

其中 etcd 是服務主檔案，etcdctl 是提供給使用者的指令用戶端，etcd-migrate 負責進行遷移。

推薦透過下面的指令將三個檔案都放到系統可執行目錄 `/usr/local/bin/` 或 `/usr/bin/`。

```
$ sudo cp etcd* /usr/local/bin/
```

執行 etcd，將 預設組建一個兩個節點的叢集。資料庫服務端 預設監聽在 2379 和 4001 連接埠，etcd 實例監聽在 2380 和 7001 連接埠。顯示類似如下的資訊：
```
$ ./etcd
2014/12/31 14:52:09 no data-dir provided, using default data-dir ./default.etcd
2014/12/31 14:52:09 etcd: listening for peers on http://localhost:2380
2014/12/31 14:52:09 etcd: listening for peers on http://localhost:7001
2014/12/31 14:52:09 etcd: listening for client requests on http://localhost:2379
2014/12/31 14:52:09 etcd: listening for client requests on http://localhost:4001
2014/12/31 14:52:09 etcdserver: name = default
2014/12/31 14:52:09 etcdserver: data dir = default.etcd
2014/12/31 14:52:09 etcdserver: snapshot count = 10000
2014/12/31 14:52:09 etcdserver: advertise client URLs = http://localhost:2379,http://localhost:4001
2014/12/31 14:52:09 etcdserver: initial advertise peer URLs = http://localhost:2380,http://localhost:7001
2014/12/31 14:52:09 etcdserver: initial cluster = default=http://localhost:2380,default=http://localhost:7001
2014/12/31 14:52:10 etcdserver: start member ce2a822cea30bfca in cluster 7e27652122e8b2ae
2014/12/31 14:52:10 raft: ce2a822cea30bfca became follower at term 0
2014/12/31 14:52:10 raft: newRaft ce2a822cea30bfca [peers: [], term: 0, commit: 0, lastindex: 0, lastterm: 0]
2014/12/31 14:52:10 raft: ce2a822cea30bfca became follower at term 1
2014/12/31 14:52:10 etcdserver: added local member ce2a822cea30bfca [http://localhost:2380 http://localhost:7001] to cluster 7e27652122e8b2ae
2014/12/31 14:52:11 raft: ce2a822cea30bfca is starting a new election at term 1
2014/12/31 14:52:11 raft: ce2a822cea30bfca became candidate at term 2
2014/12/31 14:52:11 raft: ce2a822cea30bfca received vote from ce2a822cea30bfca at term 2
2014/12/31 14:52:11 raft: ce2a822cea30bfca became leader at term 2
2014/12/31 14:52:11 raft.node: ce2a822cea30bfca elected leader ce2a822cea30bfca at term 2
2014/12/31 14:52:11 etcdserver: published {Name:default ClientURLs:[http://localhost:2379 http://localhost:4001]} to cluster 7e27652122e8b2ae
```

此時，可以使用 etcdctl 指令進行測試，設定和取得鍵值 `testkey: "hello world"`，檢查 etcd 服務是否啟動成功：
```
$ ./etcdctl set testkey "hello world"
hello world
$ ./etcdctl get testkey
hello world
```
說明 etcd 服務已經成功啟動了。

當然，也可以透過 HTTP 存取本地 2379 或 4001 連接埠的方式來進行作業，例如 檢視 `testkey` 的值：
```
$ curl -L http://localhost:4001/v2/keys/testkey
{"action":"get","node":{"key":"/testkey","value":"hello world","modifiedIndex":3,"createdIndex":3}}
```

### Docker 映像檔方式下載

映像檔名稱為 quay.io/coreos/etcd:v2.0.0_rc.1，可以透過下面的指令啟動 etcd 服務監聽到 4001 連接埠。
```
$ sudo docker run -p 4001:4001 -v /etc/ssl/certs/:/etc/ssl/certs/ quay.io/coreos/etcd:v2.0.0_rc.1
```
