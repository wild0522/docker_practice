## Compose 指令說明

### 指令對象與格式
對於 Compose 來說，大部分指令的對象既可以是項目本身，也可以指定為項目中的服務或是容器。如果沒有特別的說明，指令物件將是項目，這意味著項目中所有的服務都會受到指令影響。

執行 `docker-compose [COMMAND] --help` 或是 `docker-compose help [COMMAND]` 可以 檢視具體某個指令的使用格式。

Compose 指令的基本的使用格式是

```sh
docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]
```

### 指令選項

* `-f, --file FILE` 指定使用的 Compose  範本檔案， 預設為 `docker-compose.yml`，可以多次指定。
* `-p, --project-name NAME` 指定專案名稱， 預設將使用所在目錄名稱作為項目名。
* `--x-networking` 使用 Docker 的可拔插網路後端內容屬性（需要 Docker 1.9 及以後版本）。
* `--x-network-driver DRIVER` 指定網路後端的驅動， 預設為 `bridge`（需要 Docker 1.9 及以後版本）。
* `--verbose` 輸出更多偵錯資訊。
* `-v, --version`  列印版本並結束。

### 指令使用說明

#### `build`
格式為 `docker-compose build [options] [SERVICE...]`。

建構（重新建構）項目中的服務容器。

服務容器一旦建構後，將會帶上一個標記名，例如對於 web 項目中的一個 db 容器，可能是 web_db。

可以隨時在專案目錄下執行 `docker-compose build` 來重新建構服務。

選項包括：

* `--force-rm` 刪除建構程序中的臨時容器。
* `--no-cache` 建構映像檔程序中不使用 cache（這將加長建構程序）。
* `--pull` 始終嘗試透過 pull 來取得更新版本的映像檔。

#### `help`

獲得一個指令的說明。

#### `kill`
格式為 `docker-compose kill [options] [SERVICE...]`。

透過傳送 `SIGKILL` 信號來強制停止服務容器。

支援透過 `-s` 參數來指定傳送的信號，例如透過如下指令傳送 `SIGINT` 信號。

```sh
$ docker-compose kill -s SIGINT
```

#### `logs`
格式為 `docker-compose logs [options] [SERVICE...]`。

 檢視服務容器的輸出。 預設情況下，docker-compose 將對不同的服務輸出使用不同的色彩來區分。可以透過 `--no-color` 來關閉色彩。

該指令在偵錯問題的時候十分有用。

#### `pause`
格式為 `docker-compose pause [SERVICE...]`。

暫停一個服務容器。

#### `port`
格式為 `docker-compose port [options] SERVICE PRIVATE_PORT`。

 列印某個容器連接埠所對應的公共連接埠。

選項：

* `--protocol=proto` 指定連接埠協定，tcp（預設值）或是 udp。
* `--index=index` 如果同一服務存在多個容器，指定指令物件容器的序號（ 預設為 1）。

#### `ps`
格式為 `docker-compose ps [options] [SERVICE...]`。

列出項目中目前的所有容器。

選項：

* `-q` 只 列印容器的 ID 資訊。 

#### `pull`
格式為 `docker-compose pull [options] [SERVICE...]`。

拉取服務依賴的映像檔。

選項：

* `--ignore-pull-failures` 忽略拉取映像檔程序中的錯誤。

#### `restart`
格式為 `docker-compose restart [options] [SERVICE...]`。

重啟項目中的服務。

選項：

* `-t, --timeout TIMEOUT` 指定重啟前停止容器的逾時（ 預設為 10 秒）。

#### `rm`
格式為 `docker-compose rm [options] [SERVICE...]`。

刪除所有（停止狀態的）服務容器。推薦先執行 `docker-compose stop` 指令來停止容器。

選項：

* `-f, --force` 強制直接刪除，包括非停止狀態的容器。一般盡量不要使用這個選項。
* `-v` 刪除容器所載入的資料卷。

#### `run`
格式為 `docker-compose run [options] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]`。

在指定服務上執行一個指令。

例如：

```sh
$ docker-compose run ubuntu ping docker.com
```

將會啟動一個 ubuntu 服務容器，並執行 `ping docker.com` 指令。

 預設情況下，如果存在關聯，則所有關聯的服務將會自動被啟動，除非這些服務已經在執行中。

該指令類似啟動容器後執行指定的指令，相關卷、連結等等都將會按照設定自動建立。

兩個不同點：

* 設定指令將會覆寫原有的自動執行指令；
* 不會自動建立連接埠，以避免衝突。

如果不希望自動啟動關聯的容器，可以使用 `--no-deps` 選項，例如

```sh
$ docker-compose run --no-deps web python manage.py shell
```

將不會啟動 web 容器所關聯的其它容器。

選項：

* `-d` 後台執行容器。
* `--name NAME` 為容器指定一個名字。
* `--entrypoint CMD` 覆寫 預設的容器啟動指令。
* `-e KEY=VAL` 設定環境變數值，可多次使用選項來設定多個環境變數。
* `-u, --user=""` 指定執行容器的使用者名稱或是 uid。
* `--no-deps` 不自動啟動關聯的服務容器。
* `--rm` 執行指令後自動刪除容器，`d` 模式下將忽略。
* `-p, --publish=[]` 對應容器連接埠到本地主電腦。
* `--service-ports` 設定服務連接埠並對應到本地主電腦。
* `-T` 不指派偽 tty，意味著依賴 tty 的指令將無法執行。

#### `scale`
格式為 `docker-compose scale [options] [SERVICE=NUM...]`。

設定指定服務執行的容器個數。

透過 `service=num` 的參數來設定數量。例如：

```sh
$ docker-compose scale web=3 db=2
```

將啟動 3 個容器執行 web 服務，2 個容器執行 db 服務。

一般的，當指定數目多於該服務目前實際執行容器，將新建立並啟動容器；反之，將停止容器。

選項：

* `-t, --timeout TIMEOUT` 停止容器時候的逾時（ 預設為 10 秒）。

#### `start`
格式為 `docker-compose start [SERVICE...]`。

啟動已經存在的服務容器。

#### `stop`
格式為 `docker-compose stop [options] [SERVICE...]`。

停止已經處於執行狀態的容器，但不要刪除它。透過 `docker-compose start` 可以再次啟動這些容器。

選項：

* `-t, --timeout TIMEOUT` 停止容器時候的逾時（ 預設為 10 秒）。

#### `unpause`
格式為 `docker-compose unpause [SERVICE...]`。

恢復處於暫停狀態中的服務。

#### `up`
格式為 `docker-compose up [options] [SERVICE...]`。

該指令十分強大，它將嘗試自動完成包括建構映像檔，（重新）建立服務，啟動服務，並關聯服務相關容器的一系列作業。

連結的服務都將會被自動啟動，除非已經處於執行狀態。

可以說，大部分時候都可以直接透過該指令來啟動一個項目。

 預設情況，`docker-compose up` 啟動的容器都在前台，控制台將會同時 列印所有容器的輸出資訊，可以很方便進行偵錯。

當透過 `Ctrl-C` 停止指令時，所有容器將會停止。

如果使用 `docker-compose up -d`，將會在後台啟動並執行所有的容器。一般推薦生產環境下使用這個選項。

 預設情況，如果服務容器已經存在，`docker-compose up` 將會嘗試停止容器，然後重新建立（保持使用 `volumes-from` 載入的卷），以保證新啟動的服務符合 `docker-compose.yml` 檔案的最新內容。如果使用者不希望容器被停止並重新建立，可以使用 `docker-compose up --no-recreate`。這樣將只會啟動處於停止狀態的容器，而忽略已經執行的服務。如果使用者只想重新部署某個服務，可以使用 `docker-compose up --no-deps -d <SERVICE_NAME>` 來重新建立服務並後台停止舊服務，啟動新服務，並不會影響到其所依賴的服務。

選項：

* `-d` 在後台執行服務容器。
* `--no-color` 不使用色彩來區分不同的服務的控制台輸出。
* `--no-deps` 不啟動服務所連結的容器。
* `--force-recreate` 強制重新建立容器，不能與 `--no-recreate` 同時使用。
* `--no-recreate` 如果容器已經存在了，則不重新建立，不能與 `--force-recreate` 同時使用。
* `--no-build` 不自動建構缺失的服務映像檔。
* `-t, --timeout TIMEOUT` 停止容器時候的逾時（ 預設為 10 秒）。

#### `migrate-to-labels`
格式為 `docker-compose  migrate-to-labels`。

重新建立容器，並加入 label。

主要用於升級 1.2 及更早版本中建立的容器，加入缺失的容器標籤。

實際上，最徹底的辦法當然是刪除項目，然後重新建立。

#### `version`
格式為 `docker-compose version`。

 列印版本資訊。