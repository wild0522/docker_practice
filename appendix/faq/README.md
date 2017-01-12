# 常見問題總結

## 映像檔相關
### 如何批量清理臨時映像檔檔案
答：可以使用 `docker rmi $(docker images -q -f dangling=true)` 指令。

### 如何 檢視映像檔支援的環境變數？
答：可以使用 `docker run IMAGE env` 指令。

### 本地的映像檔檔案都存放在哪裡？
答：與 Docker 相關的本地資源都存放在 `/var/lib/docker/` 目錄下，以 aufs 檔案系統為例，其中 container 目錄存放容器資訊，graph 目錄存放映像檔資訊，aufs 目錄下存放具體的映像檔層檔案。

### 建構 Docker 映像檔應該遵循哪些原則？
答：整體原則上，盡量保持映像檔功能的明確和內容的精簡，要點包括

* 盡量選取滿足需求但較小的基礎系統映像檔，例如大部分時候可以選擇 debian:wheezy 或 debian:jessie 映像檔，僅有不足百兆大小;
* 清理編譯產生檔案、安裝包的快取等暫時檔案；
* 安裝各個軟體時候要指定準確的版本號，並避免引入不需要的依賴；
* 從安全形度考慮，應用要盡量使用系統的庫和依賴；
* 如果安裝應用時候需要設定一些特殊的環境變數，在安裝後要回存不需要保持的變數值；
* 使用 Dockerfile 建立映像檔時候要加入 .dockerignore 檔案或使用乾淨的工作目錄。

### 碰到網路問題，無法 pull 映像檔，指令行指令列指定 http_proxy 不正確？
答：在 Docker 設定檔中加入 `export http_proxy="http://<PROXY_HOST>:<PROXY_PORT>"`，之後重啟 Docker 服務即可。

## 容器相關
### 容器結束後，透過 docker ps 指令 檢視不到，資料會丟失麼？
答：容器結束後會處於終止（exited）狀態，此時可以透過 `docker ps -a`  檢視。其中的資料也不會丟失，還可以透過 `docker start` 指令來啟動它。只有刪除掉容器才會清除所有資料。

### 如何停止所有正在執行的容器
答：可以使用 `docker kill $(docker ps -q)` 指令。

### 如何清理批量後台停止的容器？
答：可以使用 `docker rm -f $(docker ps -qa)` 指令。

### 如何取得某個容器的 PID 資訊？
答：可以使用 `docker inspect --format '{{ .State.Pid }}' <CONTAINER ID or NAME>` 指令。

### 如何取得某個容器的 IP 位址？
答：可以使用 `docker inspect --format '{{ .NetworkSettings.IPAddress }}' <CONTAINER ID or NAME>` 指令

### 如何給容器指定一個固定 IP 位址，而不是每次重啟容器 IP 位址都會變？
答：目前 Docker 並沒有提供直接的對容器 IP 位址的管理支援，使用者可以考慮參考第三部分中進階網路設定章節中介紹的建立點對點連線例子，來手動組態容器的靜態 IP。或是在啟動容器後，再手動進行修改（可參考其它類的問題 「*如何進入 Docker 容器的網路命名空間？*」）。

### 如何臨時結束一個正在互動的容器的終端機，而不終止它？
答：按 `Ctrl-p Ctrl-q`。如果按 `Ctrl-c` 往往會讓容器內應用處理序終止，進而會終止容器。

### 使用 `docker port` 指令對應容器的連接埠時，系統報錯「Error: No public port '80' published for xxx」？
答：

* 建立映像檔時 Dockerfile 要透過 EXPOSE 指定正確的開放連接埠；
* 容器啟動時指定 `PublishAllPort = true`。

### 可以在一個容器中同時執行多個應用處理序麼？
答：一般並不推薦在同一個容器內執行多個應用處理序。如果有類似需求，可以透過一些額外的處理序管理機制，比如 supervisord 來管理所執行的處理序。可以參考 https://docs.docker.com/articles/using_supervisord/。

### 如何控制容器佔用系統資源（CPU、 記憶體）的份額？
答：在使用 `docker create` 指令建立容器或使用 `docker run` 建立並啟動容器的時候，可以使用 -c|--cpu-shares[=0] 參數來調整容器使用 CPU 的權重；使用 -m|--memory[=MEMORY] 參數來調整容器使用 記憶體的大小。

## 倉庫相關
### 倉庫（Repository）、註冊伺服器（Registry）、註冊索引（Index） 有何關係？
首先，倉庫是存放一組關聯映像檔的集合，比如同一個應用的不同版本的映像檔。

註冊伺服器是存放實際的映像檔檔案的地方。註冊索引則負責維護使用者的賬號、權限、搜尋、標籤等的管理。因此，註冊伺服器利用註冊索引來實作認證等管理。

### 從非官方倉庫（例如 non-official-repo.com）下載映像檔時候，有時候會提示「Error: Invalid registry endpoint https://non-official-repo.com/v1/……」？
答：Docker 自 1.3.0 版本往後，加強了對映像檔安全性的驗證，需要加入私有倉庫證書，或是手動加入對非官方倉庫的信任。
編輯 Docker 設定檔，在其中加入：

```sh
DOCKER_OPTS="--insecure-registry non-official-repo"
```
之後，重啟 Docker 服務即可。

## 設定相關
### Docker 的設定檔放在哪裡，如何修改設定？
答：使用 upstart 的系統（如Ubuntu 14.04）的設定檔在 `/etc/default/docker`，使用systemd 的系統（如 Ubuntu 16.04、Centos 等）的設定檔在 `/etc/systemd/system/docker.service.d/docker.conf`。

Ubuntu 下面的設定檔內容如下，讀者可以參考配。（如果出現該檔案不存在的情況，重啟或是自己新增一個檔案都可以解決。）

```sh
# Customize location of Docker binary (especially for development testing).
#DOCKERD="/usr/local/bin/dockerd"

# Use DOCKER_OPTS to modify the daemon startup options.
#DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

# If you need Docker to use an HTTP proxy, it can also be specified here.
#export http_proxy="http://127.0.0.1:3128/"

# This is also a handy place to tweak where Docker's temporary files go.
#export TMPDIR="/mnt/bigdrive/docker-tmp"
```

### 如何更改 Docker 的 預設儲存位置？
答：Docker 的 預設儲存位置是 `/var/lib/docker`，如果希望將 Docker 的本地檔案儲存到其它分區，可以使用 Linux 軟連線的方式來完成，或是在啟動 daemon 時透過 `-g` 參數指定。

例如，如下作業將 預設儲存位置遷移到 /storage/docker。

```sh
[root@s26 ~]# df -h
Filesystem                    Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root   50G  5.3G   42G  12% /
tmpfs                          48G  228K   48G   1% /dev/shm
/dev/sda1                     485M   40M  420M   9% /boot
/dev/mapper/VolGroup-lv_home  222G  188M  210G   1% /home
/dev/sdb2                     2.7T  323G  2.3T  13% /storage
[root@s26 ~]# service docker stop
[root@s26 ~]# cd /var/lib/
[root@s26 lib]# mv docker /storage/
[root@s26 lib]# ln -s /storage/docker/ docker
[root@s26 lib]# ls -la docker
lrwxrwxrwx. 1 root root 15 11月 17 13:43 docker -> /storage/docker
[root@s26 lib]# service docker start
```

### 使用 記憶體和 swap 限制啟動容器時候報警告：「WARNING: Your kernel does not support cgroup swap limit. WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.」？
答：這是因為系統 預設沒有開啟對 記憶體和 swap 使用的統計功能，引入該功能會帶來效能的下降。要開啟該功能，可以採取如下作業：

* 編輯 `/etc/default/grub` 檔案（Ubuntu 系統為例），設定 `GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"`；
* 更新 grub：`$ sudo update-grub`；
* 重啟系統，即可。

## Docker 與虛擬化

### Docker 與 LXC（Linux Container）有何不同？
答：LXC 利用 Linux 上相關技術實作了容器。Docker 則在如下的幾個方面進行了改進：
* 移植性：透過抽像容器設定，容器可以實作從一個平台移植到另一個平台；
* 映像檔系統：基於 AUFS 的映像檔系統為容器的分發帶來了很多的便利，同時共同的映像檔層只需要儲存一份，實作高效率的儲存；
* 版本管理：類似於Git的版本管理理念，使用者可以更方便的建立、管理映像檔檔案；
* 倉庫系統：倉庫系統大大降低了映像檔的分發和管理的成本；
* 周邊工具：各種現有工具（設定管理、雲平台）對 Docker 的支援，以及基於 Docker的 PaaS、CI 等系統，讓 Docker 的應用更加方便和多樣化。

### Docker 與 Vagrant 有何不同？
答：兩者的定位完全不同。

* Vagrant 類似 Boot2Docker（一款執行 Docker 的最小核心），是一套虛擬機的管理環境。Vagrant 可以在多種系統上和虛擬機軟體中執行，可以在 Windows，Mac 等非 Linux 平台上為 Docker 提供支援，自身具有較好的包裝性和移植性。

* 原生的 Docker 自身只能執行在 Linux 平台上，但啟動和執行的效能都比虛擬機要快，往往更適合快速開發和部署應用的場景。

簡單說：Vagrant 適合用來管理虛擬機，而 Docker 適合用來管理應用環境。

### 開發環境中 Docker 和 Vagrant 該如何選擇？
答：
Docker 不是虛擬機，而是處理序隔離，對於資源的消耗很少，但是目前需要 Linux 環境支援。Vagrant 是虛擬機上做的封裝，虛擬機本身會消耗資源。

如果本地使用的 Linux 環境，推薦都使用 Docker。

如果本地使用的是 OSX 或是 Windows 環境，那就需要開虛擬機，單一開發環境下 Vagrant 更簡單；多環境開發下推薦在 Vagrant 裡面再使用 Docker 進行環境隔離。

## 其它

### Docker 能在非 Linux 平台（比如 Windows 或 MacOS）上執行麼？
答：可以。目前需要使用 docker for mac、boot2docker 等軟體建立一個輕量級的 Linux 虛擬機層。

### 如何將一台宿主主電腦的 docker 環境遷移到另外一台宿主主電腦
答：停止 Docker 服務。將整個 Docker 儲存資料夾複製到另外一台宿主主電腦，然後調整另外一台宿主主電腦的設定即可。

### 如何進入 Docker 容器的網路命名空間？
答：Docker 在建立容器後，刪除了宿主主電腦上 `/var/run/netns` 目錄中的相關的網路命名空間檔案。因此，在宿主主電腦上是無法看到或存取容器的網路命名空間的。

使用者可以透過如下方法來手動恢復它。

首先，使用下面的指令 檢視容器處理序資訊，比如這裡的1234。

```sh
$ docker inspect --format='{{. State.Pid}} ' $container_id
1234
```
接下來，在 /proc 目錄下，把對應的網路命名空間檔案連結到 /var/run/netns 目錄。

```sh
$ sudo ln -s /proc/1234/ns/net /var/run/netns/
```

然後，在宿主主電腦上就可以看到容器的網路命名空間資訊。例如

```sh
$ sudo ip netns show
1234
```

此時，使用者可以透過正常的系統指令來 檢視或作業容器的命名空間了。例如修改容器的 IP 位址資訊為 `172.17.0.100/16`。

```sh
$ sudo ip netns exec 1234 ifconfig eth0 172.17.0.100/16
```

### 如何取得容器繫結到本地那個 veth 介面上？
答：Docker 容器啟動後，會透過 veth 介面對連線到本地橋接器，veth 介面命名跟容器命名毫無關係，十分難以找到對應關係。

最簡單的一種方式是透過 檢視介面的索引號，在容器中執行 `ip a` 指令， 檢視到本地介面最前面的介面索引號，如 `205`，將此值加上 1，即 `206`，然後在本地主電腦執行 `ip a` 指令，搜尋介面索引號為 `206` 的介面，兩者即為連線的 veth 介面對。
