## 使用
在使用 swarm 管理叢集前，需要把叢集中所有的節點的 docker daemon 的監聽方式更改為 `0.0.0.0:2375`。

可以有兩種方式達到這個目的，第一種是在啟動docker daemon的時候指定
```bash
sudo docker -H 0.0.0.0:2375&
```

第二種方式是直接修改 Docker 的設定檔(Ubuntu 上是 `/etc/default/docker`，其它版本的 Linux 上略有不同)

在檔案的最後加入下面這句程式碼：
```bash
DOCKER_OPTS="-H 0.0.0.0:2375 -H unix:///var/run/docker.sock"
```


需要注意的是，一定要在所有希望被 Swarm 管理的節點上進行的。修改之後要重啟 Docker
```bash
sudo service docker restart
```

Docker 叢集管理需要使用服務發現(Discovery service backend)功能，Swarm支援以下的幾種方式：DockerHub 提供的服務發現功能，本地的檔案，etcd，consul，zookeeper 和 IP 清單，本文會詳細講解前兩種方式，其它的使用方式都是大同小異的。

先說一下本次試驗的環境，本次試驗包括三台機器，IP位址分別為192.168.1.84,192.168.1.83和192.168.1.124.利用這三台機器組成一個docker叢集，其中83這台機器同時充當swarm manager節點。

### 使用 DockerHub 提供的服務發現功能

#### 建立叢集 token

在上面三台機器中的任何一台機器上面執行 `swarm create` 指令來取得一個叢集標誌。這條指令執行完畢後，Swarm 會移至轉到 DockerHub 上內建的發現服務中取得一個全球唯一的 token，用來識別要管理的叢集。
```bash
sudo docker run --rm swarm create
```

我們在84這台機器上執行這條指令，輸出如下：
```bash
rio@084:~$ sudo docker run --rm swarm create
b7625e5a7a2dc7f8c4faacf2b510078e
```

可以看到我們傳回的 token 是 `b7625e5a7a2dc7f8c4faacf2b510078e`，每次傳回的結果都是不一樣的。這個 token 一定要記住，後面的作業都會用到這個 token。

#### 加入叢集

在所有要加入叢集的節點上面執行 `swarm join` 指令，表示要把這台機器加入這個叢集當中。在本次試驗中，就是要在 83、84 和 124 這三台機器上執行下面的這條指令：
```bash
sudo docker run -d swarm join --addr=ip_address:2375 token://token_id
```
其中的 ip_address 換成執行這條指令的機器的 IP，token_id 換成上一步執行 `swarm create` 傳回的 token。

在83這台機器上面的執行結果如下：
```bash
rio@083:~$ sudo docker run -d swarm join --addr=192.168.1.83:2375 token://b7625e5a7a2dc7f8c4faacf2b510078e
3b3d9da603d7c121588f796eab723458af5938606282787fcbb03b6f1ac2000b
```
這條指令透過 `-d` 參數啟動了一個容器，使得83這台機器加入到叢集。如果這個容器被停止或是被刪除，83這台機器就會從叢集中消失。

#### 啟動swarm manager
因為我們要使用 83 這台機器充當 swarm 管理節點，所以需要在83這台機器上面執行 `swarm manage` 指令：
```bash
sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
```
執行結果如下：
```bash
rio@083:~$ sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
83de3e9149b7a0ef49916d1dbe073e44e8c31c2fcbe98d962a4f85380ef25f76
```
這條指令如果執行成功會傳回已經啟動的 Swarm 的容器的 ID，此時整個叢集已經啟動起來了。

現在透過 `docker ps` 指令來看下有沒有啟動成功。
```bash
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
83de3e9149b7        swarm:latest        "/swarm manage token   4 minutes ago       Up 4 minutes        0.0.0.0:2376->2375/tcp   stupefied_stallman
```
可以看到，Swarm 已經成功啟動。
在執行 `Swarm manage` 這條指令的時候，有幾點需要注意的：

* 這條指令需要在充當 swarm 管理者的機器上執行
* Swarm 要以 daemon 的形式執行
* 對應的連接埠可以使任意的除了 2375 以外的並且是未被佔用的連接埠，但一定不能是 2375 這個連接埠，因為 2375 已經被 Docker 本身給佔用了。

叢集啟動成功以後，現在我們可以在任何一台節點上使用 `swarm list` 指令 檢視叢集中的節點了，本實驗在 124 這台機器上執行 `swarm list` 指令：
```bash
rio@124:~$ sudo docker run --rm swarm list token://b7625e5a7a2dc7f8c4faacf2b510078e
192.168.1.84:2375
192.168.1.124:2375
192.168.1.83:2375
```
輸出結果列出的IP位址正是我們使用 `swarm join` 指令加入叢集的機器的IP位址。

現在我們可以在任何一台安裝了 Docker 的機器上面透過指令(指令中要指明swarm manager機器的IP位址)來在叢集中執行container了。
本次試驗，我們在 192.168.1.85 這台機器上使用 `docker info` 指令來 檢視叢集中的節點的資訊。

其中 info 也可以換成其它的 Docker 支援的指令。
```bash
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
結果輸出顯示這個叢集中只有兩個節點，IP位址分別是 192.168.1.83 和 192.168.1.84，結果不對呀，我們明明把三台機器加入了這個叢集，還有 124 這一台機器呢？
經過排查，發現是忘了修改 124 這台機器上面改 docker daemon 的監聽方式，只要按照上面的步驟修改寫 docker daemon 的監聽方式就可以了。

在使用這個方法的時候，使用swarm create可能會因為網路的原因會出現類似於下面的這個問題：
```bash
rio@227:~$ sudo docker run --rm swarm create
[sudo] password for rio:
time="2015-05-19T12:59:26Z" level=fatal msg="Post https://discovery-stage.hub.docker.com/v1/clusters: dial tcp: i/o timeout"
```

### 使用檔案

第二種方法相對於第一種方法要簡單得多，也不會出現類似於上面的問題。

第一步：在 swarm 管理節點上新增一個檔案，把要加入叢集的機器 IP 位址和連接埠號碼寫入檔案中，本次試驗就是要在83這台機器上面作業：
```bash
rio@083:~$ echo 192.168.1.83:2375 >> cluster
rio@083:~$ echo 192.168.1.84:2375 >> cluster
rio@083:~$ echo 192.168.1.124:2375 >> cluster
rio@083:~$ cat cluster
192.168.1.83:2375
192.168.1.84:2375
192.168.1.124:2375
```

第二步：在083這台機器上面執行 `swarm manage` 這條指令：
```bash
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage file:///tmp/cluster
364af1f25b776f99927b8ae26ca8db5a6fe8ab8cc1e4629a5a68b48951f598ad
```
使用`docker ps`來 檢視有沒有啟動成功：
```bash
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS              PORTS                    NAMES
364af1f25b77        swarm:latest        "/swarm manage file:   About a minute ago   Up About a minute   0.0.0.0:2376->2375/tcp   happy_euclid
```
可以看到，此時整個叢集已經啟動成功。

在使用這條指令的時候需要注意的是注意：這裡一定要使用-v指令，因為cluster檔案是在本機上面，啟動的容器 預設是存取不到的，所以要透過-v指令共享。

接下來的就可以在任何一台安裝了docker的機器上面透過指令使用叢集，同樣的，在85這台機器上執行docker info指令 檢視叢集的節點資訊：
```bash
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
