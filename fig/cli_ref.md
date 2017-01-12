##Fig用戶端參考

大部分指令都可以執行在一個或多個服務上。如果沒有特別的說明，這個指令則可以應用在所有的服務上。

執行 `fig [COMMAND] --help`  檢視所有的使用說明。

###選項

`--verbose`

顯示更多資訊。

`--version`

 列印版本並結束。

`-f, --file FILE`

使用特定的Fig檔案， 預設使用fig.yml。

`-p, --project-name NAME`

使用特定的專案名稱， 預設使用資料夾名稱。

###指令

`build`

建構或重新建構服務。

服務一旦建構後，將會標記為project_service，例如figtest_db。
如果修改服務的 `Dockerfile` 或建構目錄資訊，你可以執行 `fig build` 來重新建構。

`help`

獲得一個指令的說明。

`kill`

強制停止服務容器。

`logs`

 檢視服務的輸出。

`port`

 列印連接埠繫結的公共連接埠。

`ps`

列出所有容器。

`pull`

拉取服務映像檔。

`rm`

刪除停止的服務容器。

`run`

在一個服務上執行一個指令。

例如：

```
$ fig run web python manage.py shell
```

 預設情況下，連結的服務將會啟動，除非這些服務已經在執行中。

一次性指令會在使用與服務的普通容器相同的設定的新容器中開始執行，然後卷、連結等等都將會按照期望建立。
與普通容器唯一的不同就是，這個指令將會覆寫原有的指令，如果連接埠有衝突則不會建立。

連結還可以在一次性指令和那個服務的其它容器間建立，然後你可以像下面一樣進行一些作業：

```
$ fig run db psql -h db -U docker
```

如果你不希望在執行一次性指令時啟動連結的容器，可以指定--no-deps選項：

```
$ fig run --no-deps web python manage.py shell
```

`scale`

設定一個服務需要執行的容器個數。

透過service=num的參數來設定數量。例如：

```
$ fig scale web=2 worker=3
```

`start`

啟動一個服務已經存在的容器.

`stop`

停止一個已經執行的容器，但不要刪除它。透過 `fig start` 可以再次啟動這些容器。

`up`

建構，（重新）建立，啟動，連結一個服務的容器。

連結的服務都將會啟動，除非他們已經執行。

 預設情況， `fig up` 將會聯合供稿每個容器的輸出，而且如果容器已經存在，所有容器將會停止。如果你執行 `fig up -d` ，將會在後台啟動並執行所有的容器。

 預設情況，如果這個服務的容器已經存在， `fig up` 將會停止並重新建立他們（保持使用volumes-from載入的卷），以保證 `fig.yml` 的修改生效。如果你不想容器被停止並重新建立，可以使用 `fig up --no-recreate` 。如果需要的話，這樣將會啟動已經停止的容器。

###環境變數

環境變數可以用來設定Fig的行為。

變數以DOCKER_開頭，它們和用來設定Docker指令行指令列用戶端的使用一樣。如果你在使用 boot2docker , `$(boot2docker shellinit)` 將會設定它們為正確的值。

`FIG_PROJECT_NAME`

設定透過Fig啟動的每一個容器前加入的專案名稱. 預設是目前工作目錄的名字。

`FIG_FILE`

設定要使用的 `fig.yml` 的路徑。 預設路徑是目前工作目錄。

`DOCKER_HOST`

設定docker處理序的URL。 預設docker client使用 `unix:///var/run/docker.sock` 。

`DOCKER_TLS_VERIFY`

如果設定不為空的字型，允許和處理序進行 TLS 通信。

`DOCKER_CERT_PATH`

設定 `ca.pem` 的路徑， `cert.pem` 和 `key.pem` 檔案用來進行TLS驗證. 預設路徑是 `~/.docker` 。
