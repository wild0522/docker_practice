## Swarm 過濾器
swarm 的調度器(scheduler)在選擇節點運行容器的時候支持幾種過濾器 (filter)：Constraint,Affinity,Port,Dependency,Health

可以在執行 `swarm manage` 命令的時候通過 `--filter` 選項來設置。

###Constraint Filter
constraint 是一個跟具體節點相關聯的鍵值對，可以看做是每個節點的標籤，這個標籤可以在啟動docker daemon的時候指定，比如
```sh
sudo docker -d --label label_name=label01
```

也可以寫在docker的配置文件裡面（在ubuntu上面是 `/etc/default/docker`）。

在本次試驗中，給083添加標籤--label label_name=083,084添加標籤--label label_name=084,124添加標籤--label label_name=084,

以083為例，打開/etc/default/docker文件，修改DOCKER_OPTS：
```sh
DOCKER_OPTS="-H 0.0.0.0:2375 -H unix:///var/run/docker.sock  --label label_name=083"
```

在使用docker run命令啟動容器的時候使用 `-e constarint:key=value` 的形式，可以指定container運行的節點。

比如我們想在84上面啟動一個 redis 容器。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_1 -d -e constraint:label_name==084 redis
fee1b7b9dde13d64690344c1f1a4c3f5556835be46b41b969e4090a083a6382d
```
注意，是**兩個**等號，不是一個等號，這一點會經常被忽略。

接下來再在084這台機器上啟動一個redis 容器。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_2 -d -e constraint:label_name==084 redis         4968d617d9cd122fc2e17b3bad2f2c3b5812c0f6f51898024a96c4839fa000e1
```
然後再在083這台機器上啟動另外一個 redis 容器。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_3 -d -e constraint:label_name==083 redis         7786300b8d2232c2335ac6161c715de23f9179d30eb5c7e9c4f920a4f1d39570
```

現在來看下執行情況：
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
7786300b8d22        redis:latest        "/entrypoint.sh redi   15 minutes ago      Up 53 seconds       6379/tcp            083/redis_3
4968d617d9cd        redis:latest        "/entrypoint.sh redi   16 minutes ago      Up 2 minutes        6379/tcp            084/redis_2
fee1b7b9dde1        redis:latest        "/entrypoint.sh redi   19 minutes ago      Up 5 minutes        6379/tcp            084/redis_1
```

可以看到，執行結果跟預期的一樣。

但是如果指定一個不存在的標籤的話來運行容器會報錯。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_0 -d -e constraint:label_name==0 redis
FATA[0000] Error response from daemon: unable to find a node that satisfies label_name==0
```

###Affinity Filter
通過使用 Affinity Filter，可以讓一個容器緊挨著另一個容器啟動，也就是說讓兩個容器在同一個節點上面啟動。

現在其中一台機器上面啟動一個 redis 容器。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run -d --name redis redis
ea13eddf667992c5d8296557d3c282dd8484bd262c81e2b5af061cdd6c82158d
rio@085:~$ sudo docker -H 192.168.1.83:2376  ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS               NAMES
ea13eddf6679        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   6379/tcp            083/redis
```

然後再次啟動兩個 redis 容器。
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376  run -d --name redis_1 -e affinity:container==redis redis
bac50c2e955211047a745008fd1086eaa16d7ae4f33c192f50412e8dcd0a14cd
rio@085:~$ sudo docker -H 192.168.1.83:2376  run -d --name redis_1 -e affinity:container==redis redis
bac50c2e955211047a745008fd1086eaa16d7ae4f33c192f50412e8dcd0a14cd
```
現在來查看下運行結果,可以看到三個容器都是在一台機器上運行
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376  ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS               NAMES
449ed25ad239        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   6379/tcp            083/redis_2
bac50c2e9552        redis:latest        /entrypoint.sh redis   25 minutes ago      Up 10 seconds           6379/tcp            083/redis_1
ea13eddf6679        redis:latest        /entrypoint.sh redis   28 minutes ago      Up 3 minutes            6379/tcp            083/redis
```
通過 `-e affinity:image=image_name` 命令可以指定只有已經下載了`image_name`鏡像的機器才運行容器
```sh
sudo docker –H 192.168.1.83:2376 run –name redis1 –d –e affinity:image==redis redis 
```
redis1 這個容器只會在已經下載了 redis 鏡像的節點上運行。

```sh
sudo docker -H 192.168.1.83:2376 run -d --name redis -e affinity:image==~redis redis
```
這條命令達到的效果是：在有 redis 鏡像的節點上面啟動一個名字叫做 redis 的容器，如果每個節點上面都沒有 redis 容器，就按照預設的策略啟動 redis 容器。

###Port Filter
Port 也會被認為是一個唯一的資源
```sh
sudo docker -H 192.168.1.83:2376 run -d -p 80:80 nginx
```

執行完這條命令，之後任何使用 80 連接阜的容器都是啟動失敗。
