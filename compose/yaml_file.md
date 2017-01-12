## Compose  範本檔案

 範本檔案是使用 Compose 的核心，涉及到的指令關鍵字也比較多。但大家不用擔心，這裡面大部分指令跟 `docker run` 相關參數的含義都是類似的。

 預設的 範本檔案名稱稱為 `docker-compose.yml`，格式為 YAML 格式。

在舊版本（版本 1）中，其中每個頂級元素為服務名稱，次級元素為服務容器的設定資訊，例如

```yaml
webapp:
  image: examples/web
  ports:
    - "80:80"
  volumes:
    - "/data"
```

版本 2 延伸了 Compose 的語法，同時盡量保持跟版本 1 的相容，除了可以聲明網路和儲存資訊外，最大的不同一是加入了版本資訊，另一個是需要將所有的服務放到 `services` 根下面。

例如，上面例子改寫為版本 2，內容為

```yaml
version: "2"
services:
  webapp:
    image: examples/web
    ports:
      - "80:80"
    volumes:
      - "/data"
```


注意每個服務都必須透過 `image` 指令指定映像檔或 `build` 指令（需要 Dockerfile）等來自動建構產生映像檔。

如果使用 `build` 指令，在 `Dockerfile` 中設定的選項(例如：`CMD`, `EXPOSE`, `VOLUME`, `ENV` 等) 將會自動被取得，無需在 `docker-compose.yml` 中再次設定。

下面分別介紹各個指令的使用方式。

### `build`

指定 `Dockerfile` 所在資料夾的路徑（可以是絕對路徑，或是相對 docker-compose.yml 檔案的路徑）。 `Compose` 將會利用它自動建構這個映像檔，然後使用這個映像檔。

```
build: /path/to/build/dir
```

### `cap_add, cap_drop`
指定容器的核心能力（capacity）指派。

例如，讓容器擁有所有能力可以指定為：

```yml
cap_add:
  - ALL
```

去掉 NET_ADMIN 能力可以指定為：

```yml
cap_drop:
  - NET_ADMIN
```

### `command`

覆寫容器啟動後 預設執行的指令。

```sh
command: echo "hello world"
```

### `cgroup_parent`
指定父 cgroup 組，意味著將繼承該組的資源限制。

例如，建立了一個 cgroup 組名稱為 `cgroups_1`。

```yml
cgroup_parent: cgroups_1
```

### `container_name`
指定容器名稱。 預設將會使用 `專案名稱_服務名稱_序號` 這樣的格式。

例如：
```yml
container_name: docker-web-container
```

需要注意，指定容器名稱後，該服務將無法進行延伸（scale），因為 Docker 不允許多個容器具有相同的名稱。

### `devices`
指定裝置對應關係。

例如：
```yml
devices:
  - "/dev/ttyUSB1:/dev/ttyUSB0"
```

### `dns`

自訂 DNS 伺服器。可以是一個值，也可以是一個清單。

```sh
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

### `dns_search`

設定 DNS 搜尋網域。可以是一個值，也可以是一個清單。

```sh
dns_search: example.com
dns_search:
  - domain1.example.com
  - domain2.example.com
```

### `dockerfile`
如果需要指定額外的編譯映像檔的 Dockefile 檔案，可以透過該指令來指定。

例如
```yml
dockerfile: Dockerfile-alternate
```

注意，該指令不能跟 `image` 同時使用，否則 Compose 將不知道根據哪個指令來產生最終的服務映像檔。

### `env_file`
從檔案中取得環境變數，可以為單獨的檔案路徑或清單。

如果透過 `docker-compose -f FILE` 方式來指定 Compose  範本檔案，則 `env_file` 中變數的路徑會基於 範本檔案路徑。

如果有變數名稱與 `environment` 指令衝突，則按照慣例，以後者為準。

```sh
env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```

環境變數檔案中每一行必須符合格式，支援 `#` 開頭的註釋行。

```sh
# common.env: Set development environment
PROG_ENV=development
```

### `environment`

設定環境變數。你可以使用陣列或詞典兩種格式。

只設定名稱的變數會自動取得執行 Compose 主電腦上對應變數的值，可以用來防止洩露不必要的資料。

例如
```yml
environment:
  RACK_ENV: development
  SESSION_SECRET:
```

或是

```yml
environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

注意，如果變數名稱或是值中用到 `true|false，yes|no` 等表達布爾含義的詞彙，最好放到引號裡，避免 YAML 自動解析某些內容為對應的布爾語義。

`http://yaml.org/type/bool.html` 中給出了這些特定詞彙，包括

```sh
 y|Y|yes|Yes|YES|n|N|no|No|NO
|true|True|TRUE|false|False|FALSE
|on|On|ON|off|Off|OFF
```

### `expose`

暴露連接埠，但不對應到宿主電腦，只被連線的服務存取。

僅可以指定內部連接埠為參數

```sh
expose:
 - "3000"
 - "8000"
```

### `extends`
基於其它 範本檔案進行延伸。

例如我們已經有了一個 webapp 服務，定義一個基礎 範本檔案為 `common.yml`。
```sh
# common.yml
webapp:
  build: ./webapp
  environment:
    - DEBUG=false
    - SEND_EMAILS=false
```

再編輯一個新的 `development.yml` 檔案，使用 `common.yml` 中的 webapp 服務進行延伸。
```sh
# development.yml
web:
  extends:
    file: common.yml
    service: webapp
  ports:
    - "8000:8000"
  links:
    - db
  environment:
    - DEBUG=true
db:
  image: postgres
```
後者會自動繼承 common.yml 中的 webapp 服務及環境變數定義。

使用 extends 需要注意：

* 要避免出現重複依賴，例如 `A 依賴 B，B 依賴 C，C 反過來依賴 A` 的情況。
* extends 不會繼承 links 和 volumes_from 中定義的容器和資料卷資源。

一般的，推薦在基礎 範本中只定義一些可以共享的映像檔和環境變數，在延伸 範本中具體指定應用變數、連結、資料卷等資訊。

### `external_links`
連結到 docker-compose.yml 外部的容器，甚至 並非 `Compose` 管理的外部容器。參數格式跟 `links` 類似。

```
external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
```

### `extra_hosts`
類似 Docker 中的 `--add-host` 參數，指定額外的 host 名稱對應資訊。

例如：
```yml
extra_hosts:
 - "googledns:8.8.8.8"
 - "dockerhub:52.1.157.61"
```

會在啟動後的服務容器中 `/etc/hosts` 檔案中加入如下兩條條目。
```sh
8.8.8.8 googledns
52.1.157.61 dockerhub
```

### `image`

指定為映像檔名稱或映像檔 ID。如果映像檔在本地不存在，`Compose` 將會嘗試拉去這個映像檔。

例如：
```sh
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

### `labels`
為容器加入 Docker 元資料（metadata）資訊。例如可以為容器加入輔助說明資訊。
```yml
labels:
  com.startupteam.description: "webapp for a startup team"
  com.startupteam.department: "devops department"
  com.startupteam.release: "rc3 for v1.0"
```

### `links`

連結到其它服務中的容器。使用服務名稱（同時作為別名）或服務名稱：服務別名 `（SERVICE:ALIAS）` 格式都可以。

```sh
links:
 - db
 - db:database
 - redis
```

使用的別名將會自動在服務容器中的 `/etc/hosts` 裡建立。例如：

```sh
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

被連結容器中相應的環境變數也將被建立。

### `log_driver`
類似 Docker 中的 `--log-driver` 參數，指定日誌驅動類型。

目前支援三種日誌驅動類型。

```yml
log_driver: "json-file"
log_driver: "syslog"
log_driver: "none"
```

### `log_opt`
日誌驅動的相關參數。

例如
```yml
log_driver: "syslog"
log_opt:
  syslog-address: "tcp://192.168.0.42:123"
```

### `net`

設定網路模式。使用和 `docker client` 的 `--net` 參數一樣的值。

```sh
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

### `pid`
跟主電腦系統共享處理序命名空間。開啟這個選項的容器之間，以及容器和宿主電腦系統之間可以透過處理序 ID 來相互存取和作業。

```sh
pid: "host"
```


### `ports`

暴露連接埠資訊。

使用宿主：容器 `（HOST:CONTAINER）`格式，或是僅僅指定容器的連接埠（宿主將會隨機選擇連接埠）都可以。

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

*注意：當使用 `HOST:CONTAINER` 格式來對應連接埠時，如果你使用的容器連接埠小於 60 並且沒放到引號裡，可能會得到錯誤結果，因為 `YAML` 會自動解析 `xx:yy` 這種數碼格式為 60 進制。為避免出現這種問題，建議數碼串都採用引號包括起來的字串格式。*

### `security_opt`

指定容器 範本標籤（label）機制的 預設屬性（使用者、角色、類型、層級等）。

例如設定標籤的使用者名稱和角色名。
```yml
security_opt:
    - label:user:USER
    - label:role:ROLE
```

### `ulimits`
指定容器的 ulimits 限制值。

例如，指定最大處理序數為 65535，指定檔案控制碼數為 20000（軟限制，應用可以隨時修改，不能超過硬限制） 和 40000（系統硬限制，只能 root 使用者提高）。

```yml
  ulimits:
    nproc: 65535
    nofile:
      soft: 20000
      hard: 40000
```

### `volumes`

資料卷所載入路徑設定。可以設定宿主電腦路徑 （`HOST:CONTAINER`） 或加上存取模式 （`HOST:CONTAINER:ro`）。

該指令中路徑支援相對路徑。例如

```yml
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

### `volumes_driver`
較新版本的 Docker 支援資料卷的外掛驅動。

使用者可以先使用第三方驅動建立一個資料卷，然後使用名稱來存取它。

此時，可以透過 `volumes_driver` 來指定驅動。

```yml
volume_driver: mydriver
```

### `volumes_from`

從另一個服務或容器載入它的資料卷。

```sh
volumes_from:
 - service_name
 - container_name
```

### 其它指令

此外，還有包括 `cpu_shares, cpuset, domainname, entrypoint, hostname, ipc, mac_address, mem_limit, memswap_limit, privileged, read_only, restart, stdin_open, tty, user, working_dir` 等指令，基本跟 docker-run 中對應參數的功能一致。

例如，指定使用 cpu 核 0 和 核 1，只用 50% 的 CPU 資源：
```yml
cpu_shares: 73
cpuset: 0,1
```

指定服務容器啟動後執行的指令。
```yml
entrypoint: /code/entrypoint.sh
```

指定容器中執行應用的使用者名稱。
```yml
user: nginx
```

指定容器中工作目錄。
```yml
working_dir: /code
```

指定容器中搜尋網域名、主電腦名稱、mac 位址等。
```yml
domainname: your_website.com
hostname: test
mac_address: 08-00-27-00-0C-0A
```

指定容器中
```yml
ipc: host
```

指定容器中 記憶體和 記憶體交換區限制都為 1G。
```yml
mem_limit: 1g
memswap_limit: 1g
```

允許容器中執行一些特權指令。
```yml
privileged: true
```

指定容器結束後的重啟原則為始終重啟。該指令對保持服務始終執行十分有效，在生產環境中推薦設定為 `always` 或是 `unless-stopped`。
```yml
restart: always
```

以唯讀模式載入容器的 root 檔案系統，意味著不能對容器內容進行修改。
```yml
read_only: true
```

開啟標準輸入，可以接受外部輸入。
```yml
stdin_open: true
```

類比一個假的遠端控制台。
```yml
tty: true
```

### 讀取環境變數
從 1.5.0 版本開始，Compose  範本檔案支援動態讀取主電腦的系統環境變數。

例如，下面的 Compose 檔案將從執行它的環境中讀取變數 ${MONGO_VERSION} 的值，並寫入執行的指令中。

```yml
db:
  image: "mongo:${MONGO_VERSION}"
```

如果執行 `MONGO_VERSION=3.0 docker-compose up` 則會啟動一個 `mongo:3.2` 映像檔的容器；如果執行 `MONGO_VERSION=2.8 docker-compose up` 則會啟動一個 `mongo:2.8` 映像檔的容器。