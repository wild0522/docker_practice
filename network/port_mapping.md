## 外部存取容器
容器中可以執行一些網路應用，要讓外部也可以存取這些應用，可以透過 `-P` 或 `-p` 參數來指定連接埠對應。

當使用 -P 標記時，Docker 會隨機對應一個 `49000~49900` 的連接埠到內部容器開放的網路連接埠。

使用 `docker ps` 可以看到，本地主電腦的 49155 被對應到了容器的 5000 連接埠。此時存取本機的 49155 連接埠即可存取容器內 web 應用提供的界面。
```
$ sudo docker run -d -P training/webapp python app.py
$ sudo docker ps -l
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse
```
同樣的，可以透過 `docker logs` 指令來 檢視應用的資訊。
```
$ sudo docker logs -f nostalgic_morse
* Running on http://0.0.0.0:5000/
10.0.2.2 - - [23/May/2014 20:16:31] "GET / HTTP/1.1" 200 -
10.0.2.2 - - [23/May/2014 20:16:31] "GET /favicon.ico HTTP/1.1" 404 -
```

-p（小寫的）則可以指定要對應的連接埠，並且，在一個指定連接埠上只可以繫結一個容器。支援的格式有 `ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort`。

### 對應所有介面位址
使用 `hostPort:containerPort` 格式本地的 5000 連接埠對應到容器的 5000 連接埠，可以執行
```
$ sudo docker run -d -p 5000:5000 training/webapp python app.py
```
此時 預設會繫結本地所有介面上的所有位址。

### 對應到指定位址的指定連接埠
可以使用 `ip:hostPort:containerPort` 格式指定對應使用一個特定位址，比如 localhost 位址 127.0.0.1
```
$ sudo docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
```
### 對應到指定位址的任意連接埠
使用 `ip::containerPort` 繫結 localhost 的任意連接埠到容器的 5000 連接埠，本地主電腦會自動指定一個連接埠。
```
$ sudo docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```
還可以使用 udp 標記來指定 udp 連接埠
```
$ sudo docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
```
###  檢視對應連接埠設定
使用 `docker port` 來 檢視目前對應的連接埠設定，也可以 檢視到繫結的位址
```
$ docker port nostalgic_morse 5000
127.0.0.1:49155.
```
注意：
* 容器有自己的內部網路和 ip 位址（使用 `docker inspect` 可以取得所有的變數，Docker 還可以有一個可變的網路設定。）
* -p 標記可以多次使用來繫結多個連接埠

例如
```
$ sudo docker run -d -p 5000:5000  -p 3000:80 training/webapp python app.py
```
