## 快速設定指南

下面是一個跟 Docker 網路相關的指令清單。

其中有些指令選項只有在 Docker 服務啟動的時候才能設定，而且不能馬上生效。
* `-b BRIDGE or --bridge=BRIDGE` --指定容器載入的橋接器
* `--bip=CIDR` --訂製 docker0 的網路遮罩
* `-H SOCKET... or --host=SOCKET...` --Docker 服務端接收指令的通道
* `--icc=true|false` --是否支援容器之間進行通信
* `--ip-forward=true|false` --請看下文容器之間的通信
* `--iptables=true|false` --是否允許 Docker  加入 iptables 規則
* `--mtu=BYTES` --容器網路中的 MTU

下面2個指令選項既可以在啟動服務時指定，也可以 Docker 容器啟動（`docker run`）時候指定。在 Docker 服務啟動的時候指定則會成為預設值，後面執行 `docker run` 時可以覆寫設定的預設值。
* `--dns=IP_ADDRESS...` --使用指定的DNS伺服器
* `--dns-search=DOMAIN...` --指定DNS搜尋網域

最後這些選項只有在 `docker run` 執行時使用，因為它是針對容器的內容屬性內容。
* `-h HOSTNAME or --hostname=HOSTNAME` --設定容器主電腦名稱
* `--link=CONTAINER_NAME:ALIAS` --新增至另一個容器的連線
* `--net=bridge|none|container:NAME_or_ID|host` --設定容器的橋接模式
* `-p SPEC or --publish=SPEC` --對應容器連接埠到宿主主電腦
* `-P or --publish-all=true|false` --對應容器所有連接埠到宿主主電腦
