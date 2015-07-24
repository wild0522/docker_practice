## YAML 模板文件

預設的模板文件是 `docker-compose.yml`，其中定義的每個服務都必須通過 `image` 指令指定鏡像或 `build` 指令（需要 Dockerfile）來自動構建。

其它大部分指令都跟 `docker run` 中的類似。

如果使用 `build` 指令，在 `Dockerfile` 中設置的選項(例如：`CMD`, `EXPOSE`, `VOLUME`, `ENV` 等) 將會自動被獲取，無需在 `docker-compose.yml` 中再次設置。

### `image`

指定為鏡像名稱或鏡像 ID。如果鏡像在本地不存在，`Compose` 將會嘗試拉去這個鏡像。

例如：
```sh
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

### `build`

指定 `Dockerfile` 所在文件夾的路徑。 `Compose` 將會利用它自動構建這個鏡像，然後使用這個鏡像。

```
build: /path/to/build/dir
```

### `command`

覆蓋容器啟動後預設執行的命令。

```sh
command: bundle exec thin -p 3000
```

### `links`

鏈接到其它服務中的容器。使用服務名稱（同時作為別名）或服務名稱：服務別名 `（SERVICE:ALIAS）` 格式都可以。

```sh
links:
 - db
 - db:database
 - redis
```

使用的別名將會自動在服務容器中的 `/etc/hosts` 裡創建。例如：

```sh
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

相應的環境變量也將被創建。

### `external_links`
鏈接到 docker-compose.yml 外部的容器，甚至 並非 `Compose` 管理的容器。參數格式跟 `links` 類似。

```
external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
```


### `ports`

暴露連接阜信息。

使用宿主：容器 `（HOST:CONTAINER）`格式或者僅僅指定容器的連接阜（宿主將會隨機選擇連接阜）都可以。

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

*註：當使用 `HOST:CONTAINER` 格式來映射連接阜時，如果你使用的容器連接阜小於 60 你可能會得到錯誤得結果，因為 `YAML` 將會解析 `xx:yy` 這種數字格式為 60 進制。所以建議採用字符串格式。*


### `expose`

暴露連接阜，但不映射到宿主機，只被連接的服務訪問。

僅可以指定內部連接阜為參數

```sh
expose:
 - "3000"
 - "8000"
```

### `volumes`

卷掛載路徑設置。可以設置宿主機路徑 （`HOST:CONTAINER`） 或加上訪問模式 （`HOST:CONTAINER:ro`）。

```sh
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

### `volumes_from`

從另一個服務或容器掛載它的所有卷。

```sh
volumes_from:
 - service_name
 - container_name
```

### `environment`

設置環境變量。你可以使用數組或字典兩種格式。

只給定名稱的變量會自動獲取它在 Compose 主機上的值，可以用來防止洩露不必要的數據。

```
environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

### `env_file`
從文件中獲取環境變量，可以為單獨的文件路徑或列表。

如果通過 `docker-compose -f FILE` 指定了模板文件，則 `env_file` 中路徑會基於模板文件路徑。

如果有變量名稱與 `environment` 指令衝突，則以後者為準。

```sh
env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```

環境變量文件中每一行必須符合格式，支持 `#` 開頭的註釋行。

```sh
# common.env: Set Rails/Rack environment
RACK_ENV=development
```

### `extends`
基於已有的服務進行擴展。例如我們已經有了一個 webapp 服務，模板文件為 `common.yml`。
```sh
# common.yml
webapp:
  build: ./webapp
  environment:
    - DEBUG=false
    - SEND_EMAILS=false
```

編寫一個新的 `development.yml` 文件，使用 `common.yml` 中的 webapp 服務進行擴展。
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
後者會自動繼承 common.yml 中的 webapp 服務及相關環節變量。


### `net`

設置網路模式。使用和 `docker client` 的 `--net` 參數一樣的值。

```sh
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

### `pid`
跟主機系統共享進程命名空間。打開該選項的容器可以相互通過進程 ID 來訪問和操作。

```sh
pid: "host"
```

### `dns`

配置 DNS 服務器。可以是一個值，也可以是一個列表。

```sh
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

### `cap_add, cap_drop`
添加或放棄容器的 Linux 能力（Capabiliity）。
```sh
cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN
```

### `dns_search`

配置 DNS 搜索域。可以是一個值，也可以是一個列表。

```sh
dns_search: example.com
dns_search:
  - domain1.example.com
  - domain2.example.com
```

### `working_dir, entrypoint, user, hostname, domainname, mem_limit, privileged, restart, stdin_open, tty, cpu_shares`

這些都是和 `docker run` 支持的選項類似。

```
cpu_shares: 73

working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true

restart: always

stdin_open: true
tty: true
```
