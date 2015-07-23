## Mesos 配置項解析

Mesos 的 [配置項](http://mesos.apache.org/documentation/latest/configuration/) 可以通過啟動時候傳遞參數或者配置目錄下文件的方式給出（推薦方式，一目瞭然）。

分為三種類型：通用項（master 和 slave 都支持），只有 master 支持的，以及只有 slave 支持的。


### 通用項
* `--ip=VALUE` 監聽的 IP 地址
* `--firewall_rules=VALUE` endpoint 防火牆規則，`VALUE` 可以是 JSON 格式或者存有 JSON 格式的文件路徑。
* `--log_dir=VALUE` 日誌文件路徑，默認不存儲日誌到本地
* `--logbufsecs=VALUE`  buffer 多少秒的日誌，然後寫入本地
* `--logging_level=VALUE` 日誌記錄的最低級別
* `--port=VALUE` 監聽的端口，master 默認是 5050，slave 默認是 5051。

### master 專屬配置項
* `--quorum=VALUE` 必備項，使用基於 replicated-Log 的註冊表時，複製的個數
* `--work_dir=VALUE` 必備項，註冊表持久化信息存儲位置
* `--zk=VALUE` 必備項，zookeepr 的接口地址，支持多個地址，之間用逗號隔離，可以為文件路徑
* `--acls=VALUE` ACL 規則或所在文件
* `--allocation_interval=VALUE` 執行 allocation 的間隔，默認為 1sec
* `--allocator=VALUE` 分配機制，默認為 HierarchicalDRF
* `--[no-]authenticate` 是否允許非認證過的 framework 註冊
* `--[no-]authenticate_slaves` 是否允許非認證過的 slaves 註冊
* `--authenticators=VALUE` 對 framework 或 salves 進行認證時的實現機制
* `--cluster=VALUE` 集群別名
* `--credentials=VALUE` 存儲加密後憑證的文件的路徑
* `--external_log_file=VALUE` 採用外部的日誌文件
* `--framework_sorter=VALUE` 給定 framework 之間的資源分配策略
* `--hooks=VALUE` master 中安裝的 hook 模塊
* `--hostname=VALUE` master 節點使用的主機名，不配置則從系統中獲取
* `--[no-]log_auto_initialize` 是否自動初始化註冊表需要的 replicated 日誌
* `--modules=VALUE` 要加載的模塊，支持文件路徑或者 JSON
* `--offer_timeout=VALUE` offer 撤銷的超時
* `--rate_limits=VALUE` framework 的速率限制，比如 qps
* `--recovery_slave_removal_limit=VALUE` 限制註冊表恢復後可以移除或停止的 slave 數目，超出後 master 會失敗，默認是 100%
* `--slave_removal_rate_limit=VALUE slave` 沒有完成健康度檢查時候被移除的速率上限，例如 1/10mins 代表每十分鐘最多有一個
* `--registry=VALUE` 註冊表的持久化策略，默認為 `replicated_log`，還可以為 `in_memory`
* `--registry_fetch_timeout=VALUE` 訪問註冊表失敗超時
* `--registry_store_timeout=VALUE` 存儲註冊表失敗超時
* `--[no-]registry_strict` 是否按照註冊表中持久化信息執行操作，默認為 false
* `--roles=VALUE` 集群中 framework 可以所屬的分配角色
* `--[no-]root_submissions` root 是否可以提交 framework，默認為 true
* `--slave_reregister_timeout=VALUE` 新的 lead master 節點選舉出來後，多久之內所有的 slave 需要註冊，超時的 salve 將被移除並關閉，默認為 10mins
* `--user_sorter=VALUE` 在用戶之間分配資源的策略，默認為 drf
* `--webui_dir=VALUE` webui 實現的文件目錄所在，默認為 `/usr/local/share/mesos/webui`
* `--weights=VALUE` 各個角色的權重
* `--whitelist=VALUE` 文件路徑，包括發送 offer 的 slave 名單，默認為 None
* `--zk_session_timeout=VALUE` session 超時，默認為 10secs
* `--max_executors_per_slave=VALUE` 配置了 `--with-network-isolator` 時可用，限制每個 slave 同時執行任務個數

### slave 專屬配置項
* `--master=VALUE` 必備項，master 所在地址，或 zookeeper 地址，或文件路徑，可以是列表
* `--attributes=VALUE` 機器屬性
* `--authenticatee=VALUE` 跟 master 進行認證時候的認證機制
* `--[no-]cgroups_enable_cfs` 採用 CFS 進行帶寬限制時候對 CPU 資源進行限制，默認為 false
* `--cgroups_hierarchy=VALUE` cgroups 的目錄根位置，默認為 `/sys/fs/cgroup`
* `--[no-]cgroups_limit_swap` 限制內存和 swap，默認為 false，只限制內存
* `--cgroups_root=VALUE` 根 cgroups 的名稱，默認為 mesos
* `--container_disk_watch_interval=VALUE` 為容器進行硬盤配額查詢的時間間隔
* `--containerizer_path=VALUE` 採用外部隔離機制（`--isolation=external`）時候，外部容器機制執行文件路徑
* `--containerizers=VALUE` 可用的容器實現機制，包括 mesos、external、docker
* `--credential=VALUE` 加密後憑證，或者所在文件路徑
* `--default_container_image=VALUE` 採用外部容器機制時，任務缺省使用的鏡像
* `--default_container_info=VALUE` 容器信息的缺省值
* `--default_role=VALUE` 資源缺省分配的角色
* `--disk_watch_interval=VALUE` 硬盤使用情況的週期性檢查間隔，默認為 1mins
* `--docker=VALUE` docker 執行文件的路徑
* `--docker_remove_delay=VALUE` 刪除容器之前的等待時間，默認為 6hrs
* `--[no-]docker_kill_orphans` 清除孤兒容器，默認為 true
* `--docker_sock=VALUE` docker sock 地址，默認為 `/var/run/docker.sock`
* `--docker_mesos_image=VALUE` 運行 slave 的 docker 鏡像，如果被配置，docker 會假定 slave 運行在一個 docker 容器裡
* `--docker_sandbox_directory=VALUE` sandbox 映射到容器裡的哪個路徑
* `--docker_stop_timeout=VALUE` 停止實例後等待多久執行 kill 操作，默認為 0secs
* `--[no-]enforce_container_disk_quota` 是否啟用容器配額限制，默認為 false
* `--executor_registration_timeout=VALUE` 執行應用最多可以等多久再註冊到 slave，否則停止它，默認為 1mins
* `--executor_shutdown_grace_period=VALUE` 執行應用停止後，等待多久，默認為 5secs
* `--external_log_file=VALUE` 外部日誌文件
* `--frameworks_home=VALUE` 執行應用前添加的相對路徑，默認為空
* `--gc_delay=VALUE` 多久清理一次執行應用目錄，默認為 1weeks
* `--gc_disk_headroom=VALUE` 調整計算最大執行應用目錄年齡的硬盤留空量，默認為 0.1
* `--hadoop_home=VALUE` hadoop 安裝目錄，默認為空，會自動查找 HADOOP_HOME 或者從系統路徑中查找
* `--hooks=VALUE` 安裝在 master 中的 hook 模塊列表
* `--hostname=VALUE` slave 節點使用的主機名
* `--isolation=VALUE` 隔離機制，例如 `posix/cpu,posix/mem`（默認）或者 `cgroups/cpu,cgroups/mem`
* `--launcher_dir=VALUE` mesos 可執行文件的路徑，默認為 `/usr/local/lib/mesos`
* `--modules=VALUE` 要加載的模塊，支持文件路徑或者 JSON
* `--perf_duration=VALUE` perf 採樣時長，必須小於 perf_interval，默認為 10secs
* `--perf_events=VALUE` perf 採樣的事件
* `--perf_interval=VALUE` perf 採樣的時間間隔
* `--recover=VALUE` 回復後是否重連上舊的執行應用
* `--recovery_timeout=VALUE` slave 恢復時的超時，太久則所有相關的執行應用將自行退出，默認為 15mins
* `--registration_backoff_factor=VALUE` 跟 master 進行註冊時候的重試時間間隔算法的因子，默認為 1secs，採用隨機指數算法，最長 1mins
* `--resource_monitoring_interval=VALUE` 週期性監測執行應用資源使用情況的間隔，默認為 1secs
* `--resources=VALUE` 每個 slave 可用的資源
* `--slave_subsystems=VALUE` slave 運行在哪些 cgroup 子系統中，包括 memory，cpuacct 等，缺省為空
* `--[no-]strict` 是否認為所有錯誤都不可忽略，默認為 true
* `--[no-]switch_user` 用提交任務的用戶身份來運行，默認為 true
* `--fetcher_cache_size=VALUE` fetcher 的 cache 大小，默認為 2 GB
* `--fetcher_cache_dir=VALUE` fetcher cache 文件存放目錄，默認為 /tmp/mesos/fetch
* `--work_dir=VALUE` framework 的工作目錄，默認為 /tmp/mesos

下面的選項需要配置 `--with-network-isolator` 一起使用
* `--ephemeral_ports_per_container=VALUE` 分配給一個容器的臨時端口，默認為 1024
* `--eth0_name=VALUE` public 網絡的接口名稱，如果不指定，根據主機路由進行猜測
* `--lo_name=VALUE` loopback 網卡名稱
* `--egress_rate_limit_per_container=VALUE` 每個容器的 egress 流量限制速率
* `--[no-]network_enable_socket_statistics` 是否採集每個容器的 socket 統計信息，默認為 false
