## 使用
在使用 swarm 管理集群前，需要把集群中所有的節點的 docker daemon 的監聽方式更改為 `0.0.0.0:2375`。

可以有兩種方式達到這個目的，第一種是在啟動docker daemon的時候指定
```sh
sudo docker -H 0.0.0.0:2375&
```

第二種方式是直接修改 Docker 的配置文件(Ubuntu 上是 `/etc/default/docker`，其他版本的 Linux 上略有不同)

在文件的最後添加下面這句代碼：
```sh
DOCKER_OPTS="-H 0.0.0.0:2375 -H unix:///var/run/docker.sock"
```


需要注意的是，一定要在所有希望被 Swarm 管理的節點上進行的。修改之後要重啟 Docker
```sh
sudo service docker restart
```

Docker 集群管理需要使用服務發現(Discovery service backend)功能，Swarm支持以下的幾種方式：DockerHub 提供的服務發現功能，本地的文件，etcd，counsel，zookeeper 和 IP 列表，本文會詳細講解前兩種方式，其他的用法都是大同小異的。

先說一下本次試驗的環境，本次試驗包括三台機器，IP地址分別為192.168.1.84,192.168.1.83和192.168.1.124.利用這三台機器組成一個docker集群，其中83這台機器同時充當swarm manager節點。

### 使用 DockerHub 提供的服務發現功能

#### 創建集群 token

在上面三台機器中的任何一台機器上面執行 `swarm create` 命令來獲取一個集群標誌。這條命令執行完畢後，Swarm 會前往 DockerHub 上內置的發現服務中獲取一個全球唯一的 token，用來標識要管理的集群。
```sh
sudo docker run --rm swarm create
```

我們在84這台機器上執行這條命令，輸出如下：
```sh
rio@084:~$ sudo docker run --rm swarm create
b7625e5a7a2dc7f8c4faacf2b510078e
```

可以看到我們返回的 token 是 `b7625e5a7a2dc7f8c4faacf2b510078e`，每次返回的結果都是不一樣的。這個 token 一定要記住，後面的操作都會用到這個 token。

#### 加入集群

在所有要加入集群的節點上面執行 `swarm join` 命令，表示要把這台機器加入這個集群當中。在本次試驗中，就是要在 83、84 和 124 這三台機器上執行下面的這條命令：
```sh
sudo docker run --rm swarm join addr=ip_address:2375 token://token_id
```
其中的 ip_address 換成執行這條命令的機器的 IP，token_id 換成上一步執行 `swarm create` 返回的 token。

在83這台機器上面的執行結果如下：
```sh
rio@083:~$ sudo docker run --rm swarm join --addr=192.168.1.83:2375 token://b7625e5a7a2dc7f8c4faacf2b510078e
time="2015-05-19T11:48:03Z" level=info msg="Registering on the discovery service  every 25 seconds..." addr="192.168.1.83:2375" discovery="token://b7625e5a7a2dc7 f8c4faacf2b510078e"
```
這條命令不會自動返回，要我們自己執行 `Ctrl+C` 返回。

#### 啟動swarm manager
因為我們要使用 83 這台機器充當 swarm 管理節點，所以需要在83這台機器上面執行 `swarm manage` 命令：
```sh
sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
```
執行結果如下：
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
83de3e9149b7a0ef49916d1dbe073e44e8c31c2fcbe98d962a4f85380ef25f76
```
這條命令如果執行成功會返回已經啟動的 Swarm 的容器的 ID，此時整個集群已經啟動起來了。

現在通過 `docker ps` 命令來看下有沒有啟動成功。
```sh
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
83de3e9149b7        swarm:latest        "/swarm manage token   4 minutes ago       Up 4 minutes        0.0.0.0:2376->2375/tcp   stupefied_stallman
```
可以看到，Swarm 已經成功啟動。
在執行 `Swarm manage` 這條命令的時候，有幾點需要注意的：

* 這條命令需要在充當 swarm 管理者的機器上執行
* Swarm 要以 daemon 的形式執行
* 映射的連接阜可以使任意的除了 2375 以外的並且是未被佔用的連接阜，但一定不能是 2375 這個連接阜，因為 2375 已經被 Docker 本身給佔用了。

集群啟動成功以後，現在我們可以在任何一台節點上使用 `swarm list` 命令查看集群中的節點了，本實驗在 124 這台機器上執行 `swarm list` 命令：
```sh
rio@124:~$ sudo docker run --rm swarm list token://b7625e5a7a2dc7f8c4faacf2b510078e
192.168.1.84:2375
192.168.1.124:2375
192.168.1.83:2375
```
輸出結果列出的IP地址正是我們使用 `swarm join` 命令加入集群的機器的IP地址。

現在我們可以在任何一台安裝了 Docker 的機器上面通過命令(命令中要指明swarm manager機器的IP地址)來在集群中運行container了。
本次試驗，我們在 192.168.1.85 這台機器上使用 `docker info` 命令來查看集群中的節點的信息。

其中 info 也可以換成其他的 Docker 支持的命令。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 info
Containers: 8
Strategy: spread
Filters: affinity, health, constraint, port, dependency
Nodes: 2
 sclu083: 192.168.1.83:2375
  └ Containers: 1
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 4.054 GiB
 sclu084: 192.168.1.84:2375
  └ Containers: 7
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 4.053 GiB
```
結果輸出顯示這個集群中只有兩個節點，IP地址分別是 192.168.1.83 和 192.168.1.84，結果不對呀，我們明明把三台機器加入了這個集群，還有 124 這一台機器呢？
經過排查，發現是忘了修改 124 這台機器上面改 docker daemon 的監聽方式，只要按照上面的步驟修改寫 docker daemon 的監聽方式就可以了。

在使用這個方法的時候，使用swarm create可能會因為網路的原因會出現類似於下面的這個問題：
```sh
rio@227:~$ sudo docker run --rm swarm create
[sudo] password for rio:
time="2015-05-19T12:59:26Z" level=fatal msg="Post https://discovery-stage.hub.docker.com/v1/clusters: dial tcp: i/o timeout"
```

### 使用文件

第二種方法相對於第一種方法要簡單得多，也不會出現類似於上面的問題。

第一步：在 swarm 管理節點上新建一個文件，把要加入集群的機器 IP 地址和連接阜號寫入文件中，本次試驗就是要在83這台機器上面操作：
```sh
rio@083:~$ echo 192.168.1.83:2375 >> cluster
rio@083:~$ echo 192.168.1.84:2375 >> cluster
rio@083:~$ echo 192.168.1.124:2375 >> cluster
rio@083:~$ cat cluster
192.168.1.83:2375
192.168.1.84:2375
192.168.1.124:2375
```

第二步：在083這台機器上面執行 `swarm manage` 這條命令：
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage file:///tmp/cluster
364af1f25b776f99927b8ae26ca8db5a6fe8ab8cc1e4629a5a68b48951f598ad
```
使用`docker ps`來查看有沒有啟動成功：
```sh
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS              PORTS                    NAMES
364af1f25b77        swarm:latest        "/swarm manage file:   About a minute ago   Up About a minute   0.0.0.0:2376->2375/tcp   happy_euclid
```
可以看到，此時整個集群已經啟動成功。

在使用這條命令的時候需要注意的是注意：這裡一定要使用-v命令，因為cluster文件是在本機上面，啟動的容器預設是訪問不到的，所以要通過-v命令共享。

接下來的就可以在任何一台安裝了docker的機器上面通過命令使用集群，同樣的，在85這台機器上執行docker info命令查看集群的節點信息：
```sh
rio@s085:~$ sudo docker -H 192.168.1.83:2376 info
Containers: 9
Strategy: spread
Filters: affinity, health, constraint, port, dependency
Nodes: 3
 atsgxxx: 192.168.1.227:2375
  └ Containers: 0
  └ Reserved CPUs: 0 / 4
  └ Reserved Memory: 0 B / 2.052 GiB
 sclu083: 192.168.1.83:2375
  └ Containers: 2
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 4.054 GiB
 sclu084: 192.168.1.84:2375
  └ Containers: 7
  └ Reserved CPUs: 0 / 2
  └ Reserved Memory: 0 B / 4.053 GiB
```
