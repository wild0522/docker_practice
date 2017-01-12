##fig.yml 參考

每個在 `fig.yml` 定義的服務都需要指定一個映像檔或映像檔的建構內容。像 `docker run` 的指令行指令列一樣，其它內容是可選的。

`docker run` 在 `Dockerfile` 中設定的選項(例如：`CMD`, `EXPOSE`, `VOLUME`, `ENV`) 作為已經提供的 預設設定 - 你不需要在 `fig.yml` 中重新設定。

`image`

這裡可以設定為標籤或映像檔ID的一部分。它可以是本地的，也可以是遠端的 - 如果映像檔在本地不存在，`Fig` 將會嘗試拉去這個映像檔。

```
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

`build`

指定 `Dockerfile` 所在資料夾的路徑。 `Fig` 將會建構這個映像檔並給它產生一個名字，然後使用這個映像檔。

```
build: /path/to/build/dir
```

`command`

覆寫 預設的指令。

```
command: bundle exec thin -p 3000
```

`links`

在其它的服務中連線容器。使用服務名稱（經常也作為別名）或服務名稱加服務別名 `（SERVICE:ALIAS）` 都可以。

```
links:
 - db
 - db:database
 - redis
```

可以在服務的容器中的 `/etc/hosts` 裡建立別名。例如：

```
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

環境變數也將被建立 - 細節 檢視環境變數參考章節。

`ports`

暴露連接埠。使用宿主和容器 `（HOST:CONTAINER）` 或是僅僅容器的連接埠（宿主將會隨機選擇連接埠）都可以。

註：當使用 `HOST:CONTAINER` 格式來對應連接埠時，如果你使用的容器連接埠小於60你可能會得到錯誤得結果，因為 `YAML` 將會解析 `xx:yy` 這種數碼格式為60進制。所以我們建議用字型指定你得連接埠對應。

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

`expose`

暴露不發佈到宿主電腦的連接埠 - 它們只被連線的服務存取。僅僅內部的連接埠可以被指定。

```
expose:
 - "3000"
 - "8000"
```

`volumes`

卷載入路徑設定。可以設定宿主電腦路徑 `（HOST:CONTAINER）` 或存取模式 `（HOST:CONTAINER:ro）` 。

```
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

`volumes_from`

從另一個服務或容器載入所有卷。

```
volumes_from:
 - service_name
 - container_name
```

`environment`

設定環境變數。你可以使用陣列或詞典兩種格式。

環境變數在執行 `Fig` 的機器上被解析成一個key。它有助於安全和指定的宿主值。

```
environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

`net`

設定網路模式。使用和 `docker client` 的 `--net` 參數一樣的值。

```
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

`dns`

設定DNS伺服器。它可以是一個值，也可以是一個清單。

```
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

`working_dir, entrypoint, user, hostname, domainname, mem_limit, privileged`

這些都是和 `docker run` 對應的一個值。

```
working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true
```
