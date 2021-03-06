## swarm 調度原則
swarm支援多種調度原則來選擇節點。每次在swarm啟動container的時候，swarm會根據選擇的調度原則來選擇節點執行container。目前支援的有:spread,binpack和random。

在執行`swarm manage`指令啟動 swarm 叢集的時候可以透過 `--strategy` 參數來指定， 預設的是spread。

spread和binpack原則會根據每台節點的可用CPU， 記憶體以及正在執行的containers的數量來給各個節點分級，而random原則，顧名思義，他不會做任何的計算，只是單純的隨機選擇一個節點來啟動container。這種原則一般只做偵錯用。

使用spread原則，swarm會選擇一個正在執行的container的數量最少的那個節點來執行container。這種情況會導致啟動的container會盡可能的分佈在不同的機器上執行，這樣的好處就是如果有節點壞掉的時候不會損失太多的container。

binpack 則相反，這種情況下，swarm會盡可能的把所有的容器放在一台節點上面執行。這種原則會避免容器碎片化，因為他會把未使用的機器指派給更大的容器，帶來的好處就是swarm會使用最少的節點執行最多的容器。

### spread 原則
先來展示下 spread 原則的情況。
```bash
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage --strategy=spread file:///tmp/cluster
7609ac2e463f435c271d17887b7d1db223a5d696bf3f47f86925c781c000cb60
ats@sclu083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
7609ac2e463f        swarm:latest        "/swarm manage --str   6 seconds ago       Up 5 seconds        0.0.0.0:2376->2375/tcp   focused_babbage
```
三台機器除了83執行了 Swarm之外，其它的都沒有執行任何一個容器，現在在85這台節點上面在swarm叢集上啟動一個容器
```bash
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-1 -d -P redis
2553799f1372b432e9b3311b73e327915d996b6b095a30de3c91a47ff06ce981
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS                          NAMES
2553799f1372        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.84:32770->6379/tcp   084/node-1
```
啟動一個 redis 容器， 檢視結果
```bash

rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-2 -d -P redis
7965a17fb943dc6404e2c14fb8585967e114addca068f233fcaf60c13bcf2190
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE                            COMMAND                CREATED                  STATUS              PORTS                           NAMES
7965a17fb943        redis:latest   /entrypoint.sh redis   Less than a second ago   Up 1 seconds        192.168.1.124:49154->6379/tcp   124/node-2                  
2553799f1372        redis:latest                     /entrypoint.sh redis   29 minutes ago           Up 4 minutes        192.168.1.84:32770->6379/tcp    084/node-1
```
再次啟動一個 redis 容器， 檢視結果
```bash
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-3 -d -P redis
65e1ed758b53fbf441433a6cb47d288c51235257cf1bf92e04a63a8079e76bee
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE                            COMMAND                CREATED                  STATUS              PORTS                           NAMES
7965a17fb943        redis:latest                     /entrypoint.sh redis   Less than a second ago   Up 4 minutes        192.168.1.227:49154->6379/tcp   124/node-2
65e1ed758b53        redis:latest                     /entrypoint.sh redis   25 minutes ago           Up 17 seconds       192.168.1.83:32770->6379/tcp    083/node-3
2553799f1372        redis:latest                     /entrypoint.sh redis   33 minutes ago           Up 8 minutes        192.168.1.84:32770->6379/tcp    084/node-1
```
可以看到三個容器都是分佈在不同的節點上面的。

### binpack 原則
現在來看看binpack原則下的情況。在083上面執行指令：
```bash
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage --strategy=binpack  file:///tmp/cluster
f1c9affd5a0567870a45a8eae57fec7c78f3825f3a53fd324157011aa0111ac5
```

現在在叢集中啟動三個 redis 容器， 檢視分佈情況：
```bash
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-1 -d -P redis
18ceefa5e86f06025cf7c15919fa64a417a9d865c27d97a0ab4c7315118e348c
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-2 -d -P redis
7e778bde1a99c5cbe4701e06935157a6572fb8093fe21517845f5296c1a91bb2
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-3 -d -P redis
2195086965a783f0c2b2f8af65083c770f8bd454d98b7a94d0f670e73eea05f8
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS                          NAMES
2195086965a7        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.83:32773->6379/tcp   083/node-3
7e778bde1a99        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.83:32772->6379/tcp   083/node-2
18ceefa5e86f        redis:latest        /entrypoint.sh redis   25 minutes ago      Up 22 seconds           192.168.1.83:32771->6379/tcp   083/node-1
```

可以看到，所有的容器都是分佈在同一個節點上執行的。