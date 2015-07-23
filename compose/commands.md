## Compose 命令說明

大部分命令都可以運行在一個或多個服務上。如果沒有特別的說明，命令則應用在項目所有的服務上。

執行 `docker-compose [COMMAND] --help` 查看具體某個命令的使用說明。

基本的使用格式是
```sh
docker-compose [options] [COMMAND] [ARGS...]
```

## 選項

* `--verbose` 輸出更多調試信息。
* `--version` 打印版本並退出。
* `-f, --file FILE` 使用特定的 compose 模板文件，默認為 `docker-compose.yml`。
* `-p, --project-name NAME` 指定項目名稱，默認使用目錄名稱。

## 命令

### `build`

構建或重新構建服務。

服務一旦構建後，將會帶上一個標記名，例如 web_db。

可以隨時在項目目錄下運行 `docker-compose build` 來重新構建服務。

### `help`

獲得一個命令的幫助。

### `kill`

通過發送 `SIGKILL` 信號來強制停止服務容器。支持通過參數來指定發送的信號，例如
```sh
$ docker-compose kill -s SIGINT
```

### `logs`

查看服務的輸出。

### `port`

打印綁定的公共端口。

### `ps`

列出所有容器。

### `pull`

拉取服務鏡像。

### `rm`

刪除停止的服務容器。

### `run`

在一個服務上執行一個命令。

例如：

```
$ docker-compose run ubuntu ping docker.com
```

將會啟動一個 ubuntu 服務，執行 `ping docker.com` 命令。

默認情況下，所有關聯的服務將會自動被啟動，除非這些服務已經在運行中。

該命令類似啟動容器後運行指定的命令，相關卷、鏈接等等都將會按照期望創建。

兩個不同點：
* 給定命令將會覆蓋原有的自動運行命令；
* 不會自動創建端口，以避免衝突。

如果不希望自動啟動關聯的容器，可以使用 `--no-deps` 選項，例如

```
$ docker-compose run --no-deps web python manage.py shell
```

將不會啟動 web 容器所關聯的其它容器。

### `scale`

設置同一個服務運行的容器個數。

通過 `service=num` 的參數來設置數量。例如：

```
$ docker-compose scale web=2 worker=3
```

### `start`

啟動一個已經存在的服務容器。

### `stop`

停止一個已經運行的容器，但不刪除它。通過 `docker-compose start` 可以再次啟動這些容器。

### `up`

構建，（重新）創建，啟動，鏈接一個服務相關的容器。

鏈接的服務都將會啟動，除非他們已經運行。

默認情況， `docker-compose up` 將會整合所有容器的輸出，並且退出時，所有容器將會停止。

如果使用 `docker-compose up -d` ，將會在後台啟動並運行所有的容器。

默認情況，如果該服務的容器已經存在， `docker-compose up` 將會停止並嘗試重新創建他們（保持使用 `volumes-from` 掛載的卷），以保證 `docker-compose.yml` 的修改生效。如果你不想容器被停止並重新創建，可以使用 `docker-compose up --no-recreate`。如果需要的話，這樣將會啟動已經停止的容器。

## 環境變量

環境變量可以用來配置 Compose 的行為。

以`DOCKER_`開頭的變量和用來配置 Docker 命令行客戶端的使用一樣。如果使用 boot2docker , `$(boot2docker shellinit)` 將會設置它們為正確的值。

### `COMPOSE_PROJECT_NAME`

設置通過 Compose 啟動的每一個容器前添加的項目名稱，默認是當前工作目錄的名字。

### `COMPOSE_FILE`

設置要使用的 `docker-compose.yml` 的路徑。默認路徑是當前工作目錄。

### `DOCKER_HOST`

設置 Docker daemon 的地址。默認使用 `unix:///var/run/docker.sock`，與 Docker 客戶端採用的默認值一致。

### `DOCKER_TLS_VERIFY`

如果設置不為空，則與 Docker daemon 交互通過 TLS 進行。

### `DOCKER_CERT_PATH`

配置 TLS 通信所需要的驗證（`ca.pem`、`cert.pem` 和 `key.pem`）文件的路徑，默認是 `~/.docker` 。
