## 容器互聯
容器的連線（linking）系統是除了連接埠對應外，另一種跟容器中應用互動的方式。

該系統會在源和接收容器之間建立一個隧道，接收容器可以看到源容器指定的資訊。

### 自訂容器命名
連線系統依據容器的名稱來執行。因此，首先需要自訂一個好記的容器命名。

雖然當建立容器的時候，系統 預設會指派一個名字。自訂命名容器有2個好處：
* 自訂的命名，比較好記，比如一個web應用容器我們可以給它起名叫web
* 當要連線其它容器時候，可以作為一個有用的參考點，比如連線web容器到db容器

使用 `--name` 標記可以為容器自訂命名。
```
$ sudo docker run -d -P --name web training/webapp python app.py
```

使用 `docker ps` 來驗證設定的命名。
```
$ sudo docker ps -l
CONTAINER ID  IMAGE                  COMMAND        CREATED       STATUS       PORTS                    NAMES
aed84ee21bde  training/webapp:latest python app.py  12 hours ago  Up 2 seconds 0.0.0.0:49154->5000/tcp  web
```
也可以使用 `docker inspect` 來 檢視容器的名字
```
$ sudo docker inspect -f "{{ .Name }}" aed84ee21bde
/web
```
注意：容器的名稱是唯一的。如果已經命名了一個叫 web 的容器，當你要再次使用 web 這個名稱的時候，需要先用`docker rm` 來刪除之前建立的同名容器。

在執行 `docker run` 的時候如果加入 `--rm` 標記，則容器在終止後會立刻刪除。注意，`--rm` 和 `-d` 參數不能同時使用。

###容器互聯
使用 `--link` 參數可以讓容器之間安全的進行互動。

下面先建立一個新的資料庫容器。
```
$ sudo docker run -d --name db training/postgres
```
刪除之前建立的 web 容器
```
$ docker rm -f web
```
然後建立一個新的 web 容器，並將它連線到 db 容器
```
$ sudo docker run -d -P --name web --link db:db training/webapp python app.py
```
此時，db 容器和 web 容器建立互聯關係。

`--link` 參數的格式為 `--link name:alias`，其中 `name` 是要連結的容器的名稱，`alias` 是這個連線的別名。

使用 `docker ps` 來 檢視容器的連線
```
$ docker ps
CONTAINER ID  IMAGE                     COMMAND               CREATED             STATUS             PORTS                    NAMES
349169744e49  training/postgres:latest  su postgres -c '/usr  About a minute ago  Up About a minute  5432/tcp                 db, web/db
aed84ee21bde  training/webapp:latest    python app.py         16 hours ago        Up 2 minutes       0.0.0.0:49154->5000/tcp  web
```
可以看到自訂命名的容器，db 和 web，db 容器的 names 列有 db 也有 web/db。這表示 web 容器連結到 db 容器，web 容器將被允許存取 db 容器的資訊。

Docker 在兩個互聯的容器之間建立了一個安全隧道，而且不用對應它們的連接埠到宿主主電腦上。在啟動 db 容器的時候並沒有使用 `-p` 和 `-P` 標記，從而避免了暴露資料庫連接埠到外部網路上。

Docker 透過 2 種方式為容器公開連線資訊：
* 環境變數
* 更新 `/etc/hosts` 檔案

使用 `env` 指令來 檢視 web 容器的環境變數
```
$ sudo docker run --rm --name web2 --link db:db training/webapp env
. . .
DB_NAME=/web2/db
DB_PORT=tcp://172.17.0.5:5432
DB_PORT_5000_TCP=tcp://172.17.0.5:5432
DB_PORT_5000_TCP_PROTO=tcp
DB_PORT_5000_TCP_PORT=5432
DB_PORT_5000_TCP_ADDR=172.17.0.5
. . .
```
其中 DB_ 開頭的環境變數是供 web 容器連線 db 容器使用，前置詞採用大寫的連線別名。

除了環境變數，Docker 還加入 host 資訊到父容器的 `/etc/hosts` 的檔案。下面是父容器 web 的 hosts 檔案
```
$ sudo docker run -t -i --rm --link db:db training/webapp /bin/bash
root@aed84ee21bde:/opt/webapp# cat /etc/hosts
172.17.0.7  aed84ee21bde
. . .
172.17.0.5  db
```
這裡有 2 個 hosts，第一個是 web 容器，web 容器用 id 作為他的主電腦名稱，第二個是 db 容器的 ip 和主電腦名稱。
可以在 web 容器中安裝 ping 指令來測試跟db容器的連通。
```
root@aed84ee21bde:/opt/webapp# apt-get install -yqq inetutils-ping
root@aed84ee21bde:/opt/webapp# ping db
PING db (172.17.0.5): 48 data bytes
56 bytes from 172.17.0.5: icmp_seq=0 ttl=64 time=0.267 ms
56 bytes from 172.17.0.5: icmp_seq=1 ttl=64 time=0.250 ms
56 bytes from 172.17.0.5: icmp_seq=2 ttl=64 time=0.256 ms
```
用 ping 來測試db容器，它會解析成 `172.17.0.5`。
*注意：官方的 ubuntu 映像檔 預設沒有安裝 ping，需要自行安裝。

使用者可以連結多個父容器到子容器，比如可以連結多個 web 到 db 容器上。
