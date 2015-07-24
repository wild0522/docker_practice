## Mesos + Marathon 安裝與使用

Marathon 是可以跟 Mesos 一起協作的一個 framework，用來運行持久性的應用。

### 安裝
一共需要安裝四種組件，mesos-master、marathon、zookeeper 需要安裝到所有的主節點，mseos-slave 需要安裝到從節點。

mesos 利用 zookper 來進行主節點的同步，以及從節點發現主節點的過程。

#### 源碼編譯

下載源碼
```sh
git clone https://git-wip-us.apache.org/repos/asf/mesos.git
```

安裝依賴

```sh
#jdk-7
sudo apt-get update && sudo apt-get install -y openjdk-7-jdk
#autotools
sudo apt-get install -y autoconf libtool
#Mesos dependencies.
sudo apt-get -y install build-essential python-dev python-boto libcurl4-nss-dev libsasl2-dev maven libapr1-dev libsvn-dev
```

編譯&安裝
```sh
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

$ mkdir build
$ cd build && ../configure
$ make
$ make check && make install
```

#### [軟件源安裝](https://mesosphere.com/downloads/)
以 ubuntu 系統為例。

安裝 Docker，不再贅述，可以參考 [這裡](http://yeasy.gitbooks.io/docker_practice/content/install/index.html)。

```sh
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
將基於如下鏡像：

* ZooKeeper：https://registry.hub.docker.com/u/garland/zookeeper/
* Mesos：https://registry.hub.docker.com/u/garland/mesosphere-docker-mesos-master/
* Marathon：https://registry.hub.docker.com/u/garland/mesosphere-docker-marathon/

其中 mesos-master 鏡像將作為 master 和 slave 容器使用。

導出本地機器的地址到環境變量。
```sh
HOST_IP=10.11.31.7
```

啟動 Zookeepr 容器。
```sh
docker run -d \
-p 2181:2181 \
-p 2888:2888 \
-p 3888:3888 \
garland/zookeeper
```

啟動 Mesos Master 容器。
```sh
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
```sh
docker run \
-d \
-p 8080:8080 \
garland/mesosphere-docker-marathon --master zk://${HOST_IP}:2181/mesos --zk zk://${HOST_IP}:2181/marathon
```

啟動 Mesos slave 容器。
```sh
docker run -d \
--name mesos_slave_1 \
--entrypoint="mesos-slave" \
-e "MESOS_MASTER=zk://${HOST_IP}:2181/mesos" \
-e "MESOS_LOG_DIR=/var/log/mesos" \
-e "MESOS_LOGGING_LEVEL=INFO" \
garland/mesosphere-docker-mesos-master:latest
```

接下來，可以通過訪問本地 8080 連接阜來使用 Marathon 啟動任務了。


### 配置說明

#### ZooKeepr

ZooKeepr 是一個分佈式應用的協調工具，用來管理多個 Master 節點的選舉和冗余，監聽在 2181 連接阜。

配置文件在 /etc/zookeeper/conf/ 目錄下。

首先，要修改 myid，手動為每一個節點分配一個自己的 id（1-255之間）。

zoo.cfg 是主配置文件，主要修改如下的三行（如果你啟動三個 zk 節點）。
```sh
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

主機名需要自己替換，並在 /etc/hosts 中更新。

第一個連接阜負責從節點連接到主節點的；第二個連接阜負責主節點的選舉通信。

#### Mesos

Mesos 的預設配置目錄分別為：

* /etc/mesos：共同的配置文件，最關鍵的是 zk 文件；
* /etc/mesos-master：主節點的配置，等價於啟動 mesos-master 時候的預設選項；
* /etc/mesos-slave：從節點的配置，等價於啟動 mesos-master 時候的預設選項。

###### 主節點
首先在所有節點上修改 /etc/mesos/zk，為 主節點的 zookeeper 地址列表，例如：
```sh
zk://ip1:2181,ip2:2181/mesos
```
創建 /etc/mesos-master/ip 文件，寫入主節點監聽的地址。

還可以創建 /etc/mesos-master/cluster 文件，寫入集群的別名。

之後，啟動服務：
```sh
sudo service mesos-master start
```
更多選項可以參考[這裡](http://open.mesosphere.com/reference/mesos-master/)。

###### 從節點

在從節點上，修改 /etc/mesos-slave/ip 文件，寫入跟主節點通信的地址。

之後，啟動服務。
```sh
sudo service mesos-slave start
```

更多選項可以參考[這裡](http://open.mesosphere.com/reference/mesos-slave/)。

此時，通過瀏覽器訪問本地 5050 連接阜，可以看到節點信息。

![mesos](../_images/mesos.png)

#### Marathon
啟動 marathon 服務。
```sh
sudo service marathon start
```

啟動成功後，在 mesos 的 web界面的 frameworks 標籤頁下面將能看到名稱為 marathon 的框架出現。

同時可以通過瀏覽器訪問 8080 連接阜，看到 marathon 的管理界面。

![marathon](../_images/marathon.png)

此時，可以通過界面或者 REST API 來創建一個應用，Marathon 會保持該應用的持續運行。
