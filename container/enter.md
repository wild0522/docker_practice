## 進入容器
在使用 `-d` 參數時，容器啟動後會進入後台。
某些時候需要進入容器進行作業，有很多種方法，包括使用 `docker attach` 指令或 `nsenter` 工具等。
### attach 指令
`docker attach` 是Docker內建的指令。下面範例如何使用該指令。
```
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$sudo docker attach nostalgic_hypatia
root@243c32535da7:/#
```
但是使用 `attach` 指令有時候並不方便。當多個視窗同時 attach 到同一個容器的時候，所有視窗都會同步顯示。當某個視窗因指令阻塞時,其它視窗也無法執行作業了。

### nsenter 指令
#### 安裝
`nsenter` 工具在 util-linux 包2.23版本後包含。
如果系統中 util-linux 包沒有該指令，可以按照下面的方法從原始碼安裝。
```
$ cd /tmp; curl https://www.kernel.org/pub/linux/utils/util-linux/v2.24/util-linux-2.24.tar.gz | tar -zxf-; cd util-linux-2.24;
$ ./configure --without-ncurses
$ make nsenter && sudo cp nsenter /usr/local/bin
```

#### 使用
`nsenter` 啟動一個新的shell處理序( 預設是/bin/bash), 同時會把這個新處理序切換到和目標(target)處理序相同的命名空間，這樣就相當於進入了容器內部。nsenter 要正常工作需要有 root 權限。
很不幸，Ubuntu 14.04 仍然使用的是 util-linux 2.20。安裝最新版本的 util-linux（2.29）版，請按照以下步驟：
```
$ wget https://www.kernel.org/pub/linux/utils/util-linux/v2.29/util-linux-2.29.tar.xz; tar xJvf util-linux-2.29.tar.xz
$ cd util-linux-2.29
$ ./configure --without-ncurses && make nsenter
$ sudo cp nsenter /usr/local/bin
```
為了連線到容器，你還需要找到容器的第一個處理序的 PID，可以透過下面的指令取得。
```
PID=$(docker inspect --format "{{ .State.Pid }}" <container>)
```
透過這個 PID，就可以連線到這個容器：
```
$ nsenter --target $PID --mount --uts --ipc --net --pid
```
下面給出一個完整的例子。
```
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$ PID=$(docker-pid 243c32535da7)
10981
$ sudo nsenter --target 10981 --mount --uts --ipc --net --pid
root@243c32535da7:/#
```
更簡單的，建議大家下載
[.bashrc_docker](https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker)，並將內容放到 .bashrc 中。
```
$ wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
$ echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc
```
這個檔案中定義了很多方便使用 Docker 的指令，例如 `docker-pid` 可以取得某個容器的 PID；而 `docker-enter` 可以進入容器或直接在容器內執行指令。
```
$ echo $(docker-pid <container>)
$ docker-enter <container> ls
```
