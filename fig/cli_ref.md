##Fig客戶端參考

大部分命令都可以運行在一個或多個服務上。如果沒有特別的說明，這個命令則可以應用在所有的服務上。

執行 `fig [COMMAND] --help` 查看所有的使用說明。

###選項

`--verbose`

顯示更多信息。

`--version`

打印版本並退出。

`-f, --file FILE`

使用特定的Fig文件，默認使用fig.yml。

`-p, --project-name NAME`

使用特定的項目名稱，默認使用文件夾名稱。

###命令

`build`

構建或重新構建服務。

服務一旦構建後，將會標記為project_service，例如figtest_db。
如果修改服務的 `Dockerfile` 或構建目錄信息，你可以運行 `fig build` 來重新構建。

`help`

獲得一個命令的幫助。

`kill`

強制停止服務容器。

`logs`

查看服務的輸出。

`port`

打印端口綁定的公共端口。

`ps`

列出所有容器。

`pull`

拉取服務鏡像。

`rm`

刪除停止的服務容器。

`run`

在一個服務上執行一個命令。

例如：

```
$ fig run web python manage.py shell
```

默認情況下，鏈接的服務將會啟動，除非這些服務已經在運行中。

一次性命令會在使用與服務的普通容器相同的配置的新容器中開始運行，然後卷、鏈接等等都將會按照期望創建。
與普通容器唯一的不同就是，這個命令將會覆蓋原有的命令，如果端口有衝突則不會創建。

鏈接還可以在一次性命令和那個服務的其他容器間創建，然後你可以像下面一樣進行一些操作：

```
$ fig run db psql -h db -U docker
```

如果你不希望在執行一次性命令時啟動鏈接的容器，可以指定--no-deps選項：

```
$ fig run --no-deps web python manage.py shell
```

`scale`

設置一個服務需要運行的容器個數。

通過service=num的參數來設置數量。例如：

```
$ fig scale web=2 worker=3
```

`start`

啟動一個服務已經存在的容器.

`stop`

停止一個已經運行的容器，但不刪除它。通過 `fig start` 可以再次啟動這些容器。

`up`

構建，（重新）創建，啟動，鏈接一個服務的容器。

鏈接的服務都將會啟動，除非他們已經運行。

默認情況， `fig up` 將會聚合每個容器的輸出，而且如果容器已經存在，所有容器將會停止。如果你運行 `fig up -d` ，將會在後台啟動並運行所有的容器。

默認情況，如果這個服務的容器已經存在， `fig up` 將會停止並重新創建他們（保持使用volumes-from掛載的卷），以保證 `fig.yml` 的修改生效。如果你不想容器被停止並重新創建，可以使用 `fig up --no-recreate` 。如果需要的話，這樣將會啟動已經停止的容器。

###環境變量

環境變量可以用來配置Fig的行為。

變量以DOCKER_開頭，它們和用來配置Docker命令行客戶端的使用一樣。如果你在使用 boot2docker , `$(boot2docker shellinit)` 將會設置它們為正確的值。

`FIG_PROJECT_NAME`

設置通過Fig啟動的每一個容器前添加的項目名稱.默認是當前工作目錄的名字。

`FIG_FILE`

設置要使用的 `fig.yml` 的路徑。默認路徑是當前工作目錄。

`DOCKER_HOST`

設置docker進程的URL。默認docker client使用 `unix:///var/run/docker.sock` 。

`DOCKER_TLS_VERIFY`

如果設置不為空的字符，允許和進程進行 TLS 通信。

`DOCKER_CERT_PATH`

配置 `ca.pem` 的路徑， `cert.pem` 和 `key.pem` 文件用來進行TLS驗證.默認路徑是 `~/.docker` 。
