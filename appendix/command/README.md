# Docker 指令查詢

## 基本語法
Docker 指令有兩大類，用戶端指令和服務端指令。前者是主要的作業介面，後者用來啟動 Docker daemon。

* 用戶端指令：基本指令格式為 `docker [OPTIONS] COMMAND [arg...]`；
* 服務端指令：基本指令格式為 `docker daemon [OPTIONS]`。

可以透過 `man docker` 或 `docker help` 來 檢視這些指令。

## 用戶端指令選項

* --config=""：指定用戶端設定檔， 預設為 `/.docker`；
* -D=true|false：是否使用 debug 模式。 預設不開啟；
* -H, --host=[]：指定指令對應 Docker daemon 的監聽介面，可以為 unix 通訊端（unix:///path/to/socket），檔案控制碼（fd://socketfd）或 tcp 通訊端（tcp://[host[:port]]）， 預設為 unix:///var/run/docker.sock；
* -l, --log-level="debug|info|warn|error|fatal"：指定日誌輸出層級；
* --tls=true|false：是否對 Docker daemon 啟動 TLS 安全機制， 預設為否；
* --tlscacert= /.docker/ca.pem：TLS CA 簽章的可信證書檔案路徑；
* --tlscert= /.docker/cert.pem：TLS 可信證書檔案路徑；
* --tlscert= /.docker/key.pem：TLS 密鑰檔案路徑；
* --tlsverify=true|false：啟動 TLS 校驗， 預設為否。

## daemon 指令選項
* --api-cors-header=""：CORS 頭部網域， 預設不允許 CORS，要允許任意的跨網域存取，可以指定為 「*」；
* --authorization-plugin=""：載入認證的外掛；
* -b=""：將容器載入到一個已存在的橋接器上。指定為 'none' 時則禁用容器的網路，與 --bip 選項互斥；
* --bip=""：讓動態建立的 docker0 橋接器採用設定的 CIDR 位址; 與 -b 選項互斥；
* --cgroup-parent=""：指定 cgroup 的父組， 預設 fs cgroup 驅動為 `/docker`，systemd cgroup 驅動為 `system.slice`；
* --cluster-store=""：構成叢集（如 Swarm）時，叢集鍵值資料庫服務位址；
* --cluster-advertise=""：構成叢集時，自身的被存取位址，可以為 `host:port` 或 `interface:port`；
* --cluster-store-opt=""：構成叢集時，鍵值資料庫的設定選項；
* --config-file="/etc/docker/daemon.json"：daemon 設定檔路徑；
* --containerd=""：containerd 檔案的路徑；
* -D, --debug=true|false：是否使用 Debug 模式。省略為 false；
* --default-gateway=""：容器的 IPv4 網關位址，必須在橋接器的子網段內；
* --default-gateway-v6=""：容器的 IPv6 網關位址；
* --default-ulimit=[]： 預設的 ulimit 值；
* --disable-legacy-registry=true|false：是否允許存取舊版本的映像檔倉庫伺服器；
* --dns=""：指定容器使用的 DNS 伺服器位址；
* --dns-opt=""：DNS 選項；
* --dns-search=[]：DNS 搜尋網域；
* --exec-opt=[]：執行時的執行選項；
* --exec-root=""：容器執行狀態檔案的根路徑， 預設為 `/var/run/docker`；
* --fixed-cidr=""：限定指派 IPv4 位址範圍；
* --fixed-cidr-v6=""：限定指派 IPv6 位址範圍；
* -G, --group=""：指派給 unix 通訊端的組， 預設為 `docker`；
* -g, --graph=""：Docker 執行時的根路徑， 預設為 `/var/lib/docker`；
* -H, --host=[]：指定指令對應 Docker daemon 的監聽介面，可以為 unix 通訊端（unix:///path/to/socket），檔案控制碼（fd://socketfd）或 tcp 通訊端（tcp://[host[:port]]）， 預設為 unix:///var/run/docker.sock；
* --icc=true|false：是否啟動容器間以及跟 daemon 所在主電腦的通信。 預設為 true。
* --insecure-registry=[]：允許存取設定的非安全倉庫服務；
* --ip=""：繫結容器連接埠時候的 預設 IP 位址。省略為 0.0.0.0；
* --ip-forward=true|false：是否檢查啟動在 Docker 主電腦上的啟動 IP 轉發服務， 預設開啟。注意關閉這個選項將不對系統轉發能力進行任何檢查修改；
* --ip-masq=true|false：是否進行位址偽裝，用於容器存取外部網路， 預設開啟；
* --iptables=true|false：是否允許 Docker  加入 iptables 規則。省略為 true；
* --ipv6=true|false：是否啟動 IPv6 支援， 預設關閉；
* -l, --log-level="debug|info|warn|error|fatal"：指定日誌輸出層級；
* --label="[]"：加入指定的鍵值對標注；
* --log-driver="json-file|syslog|journald|gelf|fluentd|awslogs|splunk|etwlogs|gcplogs|none"：指定日誌後端驅動， 預設為 json-file；
* --log-opt=[]：日誌後端的選項；
* --mtu=VALUE：指定容器網路的 mtu；
* -p=""：指定 daemon 的 PID 檔案路徑。省略為 `/var/run/docker.pid`；
* --raw-logs：輸出原始，未加色彩的日誌資訊；
* --registry-mirror=<scheme>://<host>：指定 `docker pull` 時使用的註冊伺服器映像檔位址；
* -s, --storage-driver=""：指定使用設定的儲存後端；
* --selinux-enabled=true|false：是否啟動 SELinux 支援。省略值為 false。SELinux 目前尚不支援 overlay 儲存驅動；
* --storage-opt=[]：驅動後端選項；
* --tls=true|false：是否對 Docker daemon 啟動 TLS 安全機制， 預設為否；
* --tlscacert= /.docker/ca.pem：TLS CA 簽章的可信證書檔案路徑；
* --tlscert= /.docker/cert.pem：TLS 可信證書檔案路徑；
* --tlscert= /.docker/key.pem：TLS 密鑰檔案路徑；
* --tlsverify=true|false：啟動 TLS 校驗， 預設為否；
* --userland-proxy=true|false：是否使用使用者態代理來實作容器間和出容器的迴環通信， 預設為 true；
* --userns-remap=default|uid:gid|user:group|user|uid：指定容器的使用者命名空間， 預設是建立新的 UID 和 GID 對應到容器內處理序。

## 用戶端指令

可以透過 `man docker-COMMAND` 或 `docker help COMMAND` 來 檢視這些指令的具體使用方式。

* attach：依附到一個正在執行的容器中；
* build：從一個 Dockerfile 建立一個映像檔；
* commit：從一個容器的修改中建立一個新的映像檔；
* cp：在容器和本地宿主系統之間複製檔案中；
* create：建立一個新容器，但並不執行它；
* diff：檢查一個容器內檔案系統的修改，包括修改和增加；
* events：從服務端取得即時的事件；
* exec：在執行的容器內執行指令；
* export：匯出容器內容為一個 tar 包；
* history：顯示一個映像檔的歷史資訊；
* images：列出存在的映像檔；
* import： 匯入一個檔案（通常為 tar 包）路徑或目錄來建立一個本地映像檔；
* info：顯示一些相關的系統資訊；
* inspect：顯示一個容器的具體設定資訊；
* kill：關閉一個執行中的容器 (包括處理序和所有相關資源)；
* load：從一個 tar 包中載入一個映像檔；
* login：註冊或登入到一個 Docker 的倉庫伺服器；
* logout：從 Docker 的倉庫伺服器登出；
* logs：取得容器的 log 資訊；
* network：管理 Docker 的網路，包括 檢視、建立、刪除、載入、卸載等；
* node：管理 swarm 叢集中的節點，包括 檢視、更新、刪除、提升/取消管理節點等；
* pause：暫停一個容器中的所有處理序；
* port：搜尋一個 nat 到一個私有連接阜的公共的連接阜；
* ps：列出主電腦上的容器；
* pull：從一個Docker的倉庫伺服器下拉一個映像檔或倉庫；
* push：將一個映像檔或是倉庫推送到一個 Docker 的註冊伺服器；
* rename：重新命名一個容器；
* restart：重啟一個執行中的容器；
* rm：刪除設定的若干個容器；
* rmi：刪除設定的若干個映像檔；
* run：建立一個新容器，並在其中執行設定指令；
* save：儲存一個映像檔為 tar 包檔案；
* search：在 Docker index 中搜尋一個映像檔；
* service：管理 Docker 所啟動的應用服務，包括建立、更新、刪除等；
* start：啟動一個容器；
* stats：輸出（一個或多個）容器的資源使用統計資訊；
* stop：終止一個執行中的容器；
* swarm：管理 Docker swarm 叢集，包括建立、加入、結束、更新等；
* tag：為一個映像檔打標籤；
* top： 檢視一個容器中的正在執行的處理序資訊；
* unpause：將一個容器內所有的處理序從暫停狀態中恢復；
* update：更新指定的若干容器的設定資訊；
* version：輸出 Docker 的版本資訊；
* volume：管理 Docker volume，包括 檢視、建立、刪除等；
* wait：阻塞直到一個容器終止，然後輸出它的結束符。

## 一張圖總結 Docker 的指令
![Docker 指令總結](../_images/cmd_logic.png)
