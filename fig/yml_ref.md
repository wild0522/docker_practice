##fig.yml 參考

每個在 `fig.yml` 定義的服務都需要指定一個鏡像或鏡像的構建內容。像 `docker run` 的命令行一樣，其它內容是可選的。

`docker run` 在 `Dockerfile` 中設置的選項(例如：`CMD`, `EXPOSE`, `VOLUME`, `ENV`) 作為已經提供的默認設置 - 你不需要在 `fig.yml` 中重新設置。

`image`

這裡可以設置為標籤或鏡像ID的一部分。它可以是本地的，也可以是遠程的 - 如果鏡像在本地不存在，`Fig` 將會嘗試拉去這個鏡像。

```
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

`build`

指定 `Dockerfile` 所在文件夾的路徑。 `Fig` 將會構建這個鏡像並給它生成一個名字，然後使用這個鏡像。

```
build: /path/to/build/dir
```

`command`

覆蓋默認的命令。

```
command: bundle exec thin -p 3000
```

`links`

在其它的服務中連接容器。使用服務名稱（經常也作為別名）或服務名稱加服務別名 `（SERVICE:ALIAS）` 都可以。

```
links:
 - db
 - db:database
 - redis
```

可以在服務的容器中的 `/etc/hosts` 裡創建別名。例如：

```
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

環境變量也將被創建 - 細節查看環境變量參考章節。

`ports`

暴露端口。使用宿主和容器 `（HOST:CONTAINER）` 或者僅僅容器的端口（宿主將會隨機選擇端口）都可以。

註：當使用 `HOST:CONTAINER` 格式來映射端口時，如果你使用的容器端口小於60你可能會得到錯誤得結果，因為 `YAML` 將會解析 `xx:yy` 這種數字格式為60進制。所以我們建議用字符指定你得端口映射。

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

`expose`

暴露不發佈到宿主機的端口 - 它們只被連接的服務訪問。僅僅內部的端口可以被指定。

```
expose:
 - "3000"
 - "8000"
```

`volumes`

卷掛載路徑設置。可以設置宿主機路徑 `（HOST:CONTAINER）` 或訪問模式 `（HOST:CONTAINER:ro）` 。

```
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

`volumes_from`

從另一個服務或容器掛載所有卷。

```
volumes_from:
 - service_name
 - container_name
```

`environment`

設置環境變量。你可以使用數組或字典兩種格式。

環境變量在運行 `Fig` 的機器上被解析成一個key。它有助於安全和指定的宿主值。

```
environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

`net`

設置網絡模式。使用和 `docker client` 的 `--net` 參數一樣的值。

```
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

`dns`

配置DNS服務器。它可以是一個值，也可以是一個列表。

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
