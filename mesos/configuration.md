## Mesos 設定項解析

Mesos 的 [設定項](http://mesos.apache.org/documentation/latest/configuration/) 可以透過啟動時候傳遞參數或是設定目錄下檔案的方式給出（推薦方式，一目瞭然）。

分為三種類型：泛用項（master 和 slave 都支援），只有 master 支援的，以及只有 slave 支援的。


### 泛用項
* `--ip=VALUE` 監聽的 IP 位址
* `--firewall_rules=VALUE` endpoint 防火牆規則，`VALUE` 可以是 JSON 格式或是存有 JSON 格式的檔案路徑。
* `--log_dir=VALUE` 記錄檔路徑， 預設不儲存日誌到本地
* `--logbufsecs=VALUE`  buffer 多少秒的日誌，然後寫入本地
* `--logging_level=VALUE` 日誌記錄的最低層級
* `--port=VALUE` 監聽的連接埠，master  預設是 5050，slave  預設是 5051。

### master 專屬設定項
* `--quorum=VALUE` 必備項，使用基於 replicated-Log 的    註冊表  時，複製的個數
* `--work_dir=VALUE` 必備項，    註冊表  持久化資訊儲存位置
* `--zk=VALUE` 必備項，zookeepr 的介面位址，支援多個位址，之間用逗號隔離，可以為檔案路徑
* `--acls=VALUE` ACL 規則或所在檔案
* `--allocation_interval=VALUE` 執行 allocation 的間隔， 預設為 1sec
* `--allocator=VALUE` 指派機制， 預設為 HierarchicalDRF
* `--[no-]authenticate` 是否允許非認證過的 framework 註冊
* `--[no-]authenticate_slaves` 是否允許非認證過的 slaves 註冊
* `--authenticators=VALUE` 對 framework 或 salves 進行認證時的實作機制
* `--cluster=VALUE` 叢集別名
* `--credentials=VALUE` 儲存加密後憑證的檔案的路徑
* `--external_log_file=VALUE` 採用外部的記錄檔
* `--framework_sorter=VALUE` 設定 framework 之間的資源指派原則
* `--hooks=VALUE` master 中安裝的 hook 模組
* `--hostname=VALUE` master 節點使用的主電腦名稱，不設定則從系統中取得
* `--[no-]log_auto_initialize` 是否自動起始    註冊表  需要的 replicated 日誌
* `--modules=VALUE` 要載入的模組，支援檔案路徑或是 JSON
* `--offer_timeout=VALUE` offer 復原的逾時
* `--rate_limits=VALUE` framework 的速率限制，比如 qps
* `--recovery_slave_removal_limit=VALUE` 限制    註冊表  恢復後可以移除或停止的 slave 數目，超出後 master 會失敗， 預設是 100%
* `--slave_removal_rate_limit=VALUE slave` 沒有完成健康度檢查時候被移除的速率上限，例如 1/10mins 代表每十分鐘最多有一個
* `--registry=VALUE`     註冊表  的持久化原則， 預設為 `replicated_log`，還可以為 `in_memory`
* `--registry_fetch_timeout=VALUE` 存取    註冊表  失敗逾時
* `--registry_store_timeout=VALUE` 儲存    註冊表  失敗逾時
* `--[no-]registry_strict` 是否按照    註冊表  中持久化資訊執行作業， 預設為 false
* `--roles=VALUE` 叢集中 framework 可以所屬的指派角色
* `--[no-]root_submissions` root 是否可以送出 framework， 預設為 true
* `--slave_reregister_timeout=VALUE` 新的 lead master 節點選舉出來後，多久之內所有的 slave 需要註冊，逾時的 salve 將被移除並關閉， 預設為 10mins
* `--user_sorter=VALUE` 在使用者之間指派資源的原則， 預設為 drf
* `--webui_dir=VALUE` webui 實作的檔案目錄所在， 預設為 `/usr/local/share/mesos/webui`
* `--weights=VALUE` 各個角色的權重
* `--whitelist=VALUE` 檔案路徑，包括傳送 offer 的 slave 名單， 預設為 None
* `--zk_session_timeout=VALUE` session 逾時， 預設為 10secs
* `--max_executors_per_slave=VALUE` 設定了 `--with-network-isolator` 時可用，限制每個 slave 同時執行工作個數

### slave 專屬設定項
* `--master=VALUE` 必備項，master 所在位址，或 zookeeper 位址，或檔案路徑，可以是清單
* `--attributes=VALUE` 機器屬性
* `--authenticatee=VALUE` 跟 master 進行認證時候的認證機制
* `--[no-]cgroups_enable_cfs` 採用 CFS 進行頻寬限制時候對 CPU 資源進行限制， 預設為 false
* `--cgroups_hierarchy=VALUE` cgroups 的目錄根位置， 預設為 `/sys/fs/cgroup`
* `--[no-]cgroups_limit_swap` 限制 記憶體和 swap， 預設為 false，只限制 記憶體
* `--cgroups_root=VALUE` 根 cgroups 的名稱， 預設為 mesos
* `--container_disk_watch_interval=VALUE` 為容器進行硬碟配額查詢的時間間隔
* `--containerizer_path=VALUE` 採用外部隔離機制（`--isolation=external`）時候，外部容器機制執行檔案路徑
* `--containerizers=VALUE` 可用的容器實作機制，包括 mesos、external、docker
* `--credential=VALUE` 加密後憑證，或是所在檔案路徑
* `--default_container_image=VALUE` 採用外部容器機制時，工作省略使用的映像檔
* `--default_container_info=VALUE` 容器資訊的省略值
* `--default_role=VALUE` 資源省略指派的角色
* `--disk_watch_interval=VALUE` 硬碟使用情況的週期性檢查間隔， 預設為 1mins
* `--docker=VALUE` docker 執行檔案的路徑
* `--docker_remove_delay=VALUE` 刪除容器之前的等待時間， 預設為 6hrs
* `--[no-]docker_kill_orphans` 清除孤兒容器， 預設為 true
* `--docker_sock=VALUE` docker sock 位址， 預設為 `/var/run/docker.sock`
* `--docker_mesos_image=VALUE` 執行 slave 的 docker 映像檔，如果被設定，docker 會假定 slave 執行在一個 docker 容器裡
* `--docker_sandbox_directory=VALUE` sandbox 對應到容器裡的哪個路徑
* `--docker_stop_timeout=VALUE` 停止實例後等待多久執行 kill 作業， 預設為 0secs
* `--[no-]enforce_container_disk_quota` 是否啟動容器配額限制， 預設為 false
* `--executor_registration_timeout=VALUE` 執行應用最多可以等多久再註冊到 slave，否則停止它， 預設為 1mins
* `--executor_shutdown_grace_period=VALUE` 執行應用停止後，等待多久， 預設為 5secs
* `--external_log_file=VALUE` 外部記錄檔
* `--frameworks_home=VALUE` 執行應用前加入的相對路徑， 預設為空
* `--gc_delay=VALUE` 多久清理一次執行應用目錄， 預設為 1weeks
* `--gc_disk_headroom=VALUE` 調整計算最大執行應用目錄年齡的硬碟留空量， 預設為 0.1
* `--hadoop_home=VALUE` hadoop 安裝目錄， 預設為空，會自動搜尋 HADOOP_HOME 或是從系統路徑中搜尋
* `--hooks=VALUE` 安裝在 master 中的 hook 模組清單
* `--hostname=VALUE` slave 節點使用的主電腦名稱
* `--isolation=VALUE` 隔離機制，例如 `posix/cpu,posix/mem`（ 預設）或是 `cgroups/cpu,cgroups/mem`
* `--launcher_dir=VALUE` mesos 可執行檔案的路徑， 預設為 `/usr/local/lib/mesos`
* `--modules=VALUE` 要載入的模組，支援檔案路徑或是 JSON
* `--perf_duration=VALUE` perf 採樣時長，必須小於 perf_interval， 預設為 10secs
* `--perf_events=VALUE` perf 採樣的事件
* `--perf_interval=VALUE` perf 採樣的時間間隔
* `--recover=VALUE` 回復後是否重連上舊的執行應用
* `--recovery_timeout=VALUE` slave 恢復時的逾時，太久則所有相關的執行應用將自行結束， 預設為 15mins
* `--registration_backoff_factor=VALUE` 跟 master 進行註冊時候的重試時間間隔算法的因子， 預設為 1secs，採用隨機指數算法，最長 1mins
* `--resource_monitoring_interval=VALUE` 週期性監測執行應用資源使用情況的間隔， 預設為 1secs
* `--resources=VALUE` 每個 slave 可用的資源
* `--slave_subsystems=VALUE` slave 執行在哪些 cgroup 子系統中，包括 memory，cpuacct 等，省略為空
* `--[no-]strict` 是否認為所有錯誤都不可忽略， 預設為 true
* `--[no-]switch_user` 用送出工作的使用者身份來執行， 預設為 true
* `--fetcher_cache_size=VALUE` fetcher 的 cache 大小， 預設為 2 GB
* `--fetcher_cache_dir=VALUE` fetcher cache 檔案存放目錄， 預設為 /tmp/mesos/fetch
* `--work_dir=VALUE` framework 的工作目錄， 預設為 /tmp/mesos

下面的選項需要設定 `--with-network-isolator` 一起使用
* `--ephemeral_ports_per_container=VALUE` 指派給一個容器的臨時連接埠， 預設為 1024
* `--eth0_name=VALUE` public 網路的介面名稱，如果不指定，根據主電腦路由進行猜測
* `--lo_name=VALUE` loopback 網卡名稱
* `--egress_rate_limit_per_container=VALUE` 每個容器的 egress 流量限制速率
* `--[no-]network_enable_socket_statistics` 是否採集每個容器的 socket 統計資訊， 預設為 false
