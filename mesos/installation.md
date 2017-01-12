## Mesos + Marathon 安裝與使用

Marathon 是可以跟 Mesos 一起協作的一個 framework，用來執行持久性的應用。

### 安裝
一共需要安裝四種元件，mesos-master、marathon、zookeeper 需要安裝到所有的主節點，mseos-slave 需要安裝到從節點。

mesos 利用 zookeeper 來進行主節點的同步，以及從節點發現主節點的程序。

#### 原始碼編譯

下載原始碼
```bash
git clone https://git-wip-us.apache.org/repos/asf/mesos.git
```

安裝依賴

```bash
#jdk-7
sudo apt-get update && sudo apt-get install -y openjdk-7-jdk
#autotools
sudo apt-get install -y autoconf libtool
#Mesos dependencies.
sudo apt-get -y install build-essential python-dev python-boto libcurl4-nss-dev libsasl2-dev maven libapr1-dev libsvn-dev
```

編譯&安裝
```bash
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

$ mkdir build
$ cd build && ../configure
$ make
$ make check && make install
```

#### [軟體源安裝](https://mesosphere.com/downloads/)
以 ubuntu 系統為例。

安裝 Docker，不再贅述，可以參考 [這裡](http://yeasy.gitbooks.io/docker_practice/content/install/index.html)。

```bash
# Setup
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
CODENAME=$(lsb_release -cs)

# Add the repository
echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | \
  sudo tee /etc/apt/sources.list.d/mesosphere.list

sudo apt-get -y update && sudo apt-get -y install zookeeper mesos marathon
```

#### [基於 Docker](https://github.com/sekka1/mesosphere-docker)
將基於如下映像檔：

* ZooKeeper：https://registry.hub.docker.com/u/garland/zookeeper/
* Mesos：https://registry.hub.docker.com/u/garland/mesosphere-docker-mesos-master/
* Marathon：https://registry.hub.docker.com/u/garland/mesosphere-docker-marathon/

其中 mesos-master 映像檔將作為 master 和 slave 容器使用。

匯出本地機器的位址到環境變數。
```bash
HOST_IP=10.11.31.7
```

啟動 Zookeepr 容器。
```bash
docker run -d \
-p 2181:2181 \
-p 2888:2888 \
-p 3888:3888 \
garland/zookeeper
```

啟動 Mesos Master 容器。
```bash
docker run --net="host" \
-p 5050:5050 \
-e "MESOS_HOSTNAME=${HOST_IP}" \
-e "MESOS_IP=${HOST_IP}" \
-e "MESOS_ZK=zk://${HOST_IP}:2181/mesos" \
-e "MESOS_PORT=5050" \
-e "MESOS_LOG_DIR=/var/log/mesos" \
-e "MESOS_QUORUM=1" \
-e "MESOS_REGISTRY=in_memory" \
-e "MESOS_WORK_DIR=/var/lib/mesos" \
-d \
garland/mesosphere-docker-mesos-master
```

啟動 Marathon。
```bash
docker run \
-d \
-p 8080:8080 \
garland/mesosphere-docker-marathon --master zk://${HOST_IP}:2181/mesos --zk zk://${HOST_IP}:2181/marathon
```

啟動 Mesos slave 容器。
```bash
docker run -d \
--name mesos_slave_1 \
--entrypoint="mesos-slave" \
-e "MESOS_MASTER=zk://${HOST_IP}:2181/mesos" \
-e "MESOS_LOG_DIR=/var/log/mesos" \
-e "MESOS_LOGGING_LEVEL=INFO" \
garland/mesosphere-docker-mesos-master:latest
```

接下來，可以透過存取本地 8080 連接埠來使用 Marathon 啟動工作了。


### 設定說明

#### ZooKeepr

ZooKeepr 是一個分散式應用的協調工具，用來管理多個 Master 節點的選舉和冗余，監聽在 2181 連接埠。

設定檔在 /etc/zookeeper/conf/ 目錄下。

首先，要修改 myid，手動為每一個節點指派一個自己的 id（1-255之間）。

zoo.cfg 是主設定檔，主要修改如下的三行（如果你啟動三個 zk 節點）。
```bash
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

主電腦名稱需要自己取代，並在 /etc/hosts 中更新。

第一個連接埠負責從節點連線到主節點的；第二個連接埠負責主節點的選舉通信。

#### Mesos

Mesos 的 預設設定目錄分別為：

* /etc/mesos：共同的設定檔，最關鍵的是 zk 檔案；
* /etc/mesos-master：主節點的設定，等價於啟動 mesos-master 時候的 預設選項；
* /etc/mesos-slave：從節點的設定，等價於啟動 mesos-master 時候的 預設選項。

###### 主節點
首先在所有節點上修改 /etc/mesos/zk，為 主節點的 zookeeper 位址清單，例如：
```bash
zk://ip1:2181,ip2:2181/mesos
```
建立 /etc/mesos-master/ip 檔案，寫入主節點監聽的位址。

還可以建立 /etc/mesos-master/cluster 檔案，寫入叢集的別名。

之後，啟動服務：
```bash
sudo service mesos-master start
```
更多選項可以參考[這裡](http://open.mesosphere.com/reference/mesos-master/)。

###### 從節點

在從節點上，修改 /etc/mesos-slave/ip 檔案，寫入跟主節點通信的位址。

之後，啟動服務。
```bash
sudo service mesos-slave start
```

更多選項可以參考[這裡](http://open.mesosphere.com/reference/mesos-slave/)。

此時，透過瀏覽器存取本地 5050 連接埠，可以看到節點資訊。

![mesos](../_images/mesos.png)

#### Marathon
啟動 marathon 服務。
```bash
sudo service marathon start
```

啟動成功後，在 mesos 的 web界面的 frameworks 標籤頁下面將能看到名稱為 marathon 的框架出現。

同時可以透過瀏覽器存取 8080 連接埠，看到 marathon 的管理界面。

![marathon](../_images/marathon.png)

此時，可以透過界面或是 REST API 來建立一個應用，Marathon 會保持該應用的持續執行。
