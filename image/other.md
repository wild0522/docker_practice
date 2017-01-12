## 其它產生映像檔的方法

除了標準的使用 `Dockerfile` 產生映像檔的方法外，由於各種特殊需求和歷史原因，還提供了一些其它方法用以產生映像檔。

### 從 rootfs 壓縮包 匯入

格式：`docker import [選項] <檔案>|<URL>|- [<倉庫名>[:<標籤>]]`

壓縮包可以是本地檔案、遠端 Web 檔案，甚至是從標準輸入中得到。壓縮包將會在映像檔 `/` 目錄展開，並直接作為映像檔第一層送出。

比如我們想要建立一個 [OpenVZ](https://openvz.org/Main_Page) 的 Ubuntu 14.04 [ 範本](https://openvz.org/Download/template/precreated)的映像檔：

```bash
$ docker import \
    http://download.openvz.org/template/precreated/ubuntu-14.04-x86_64-minimal.tar.gz \
    openvz/ubuntu:14.04
Downloading from http://download.openvz.org/template/precreated/ubuntu-14.04-x86_64-minimal.tar.gz
sha256:f477a6e18e989839d25223f301ef738b69621c4877600ae6467c4e5289822a79B/78.42 MB
```

這條指令自動下載了 `ubuntu-14.04-x86_64-minimal.tar.gz` 檔案，並且作為根檔案系統展開 匯入，並儲存為映像檔 `openvz/ubuntu:14.04`。

 匯入成功後，我們可以用 `docker ps` 看到這個 匯入的映像檔：

```bash
$ docker images openvz/ubuntu
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
openvz/ubuntu       14.04               f477a6e18e98        55 seconds ago      214.9 MB
```

如果我們 檢視其歷史的話，會看到描述中有 匯入的檔案連結：

```bash
$ docker history openvz/ubuntu:14.04
IMAGE               CREATED              CREATED BY          SIZE                COMMENT
f477a6e18e98        About a minute ago                       214.9 MB            Imported from http://download.openvz.org/template/precreated/ubuntu-14.04-x86_64-minimal.tar.gz
```

### `docker save` 和 `docker load`

Docker 還提供了 `docker load` 和 `docker save` 指令，用以將映像檔儲存為一個 `tar` 檔案，然後傳輸到另一個位置上，在載入進來。這是在沒有 Docker Registry 時的做法，現在已經不推薦，映像檔遷移應該直接使用 Docker Registry，無論是直接使用 Docker Hub 還是使用內網私有 Registry 都可以。

#### 儲存映像檔

使用 `docker save` 指令可以將映像檔儲存為歸檔檔案。

比如我們希望儲存這個 `alpine` 映像檔。

```bash
$ docker images alpine
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              baa5d63471ea        5 weeks ago         4.803 MB
```

儲存映像檔的指令為：

```bash
$ docker save alpine | gzip > alpine-latest.tar.gz
```

然後我們將 `alpine-latest.tar.gz` 檔案複製到了到了另一個機器上，可以用下面這個指令載入映像檔：

```bash
$ docker load -i alpine-latest.tar.gz
Loaded image: alpine:latest
```

如果我們結合這兩個指令以及 `ssh` 甚至 `pv` 的話，利用 Linux 強大的管道，我們可以寫一個指令完成從一個機器將映像檔遷移到另一個機器，並且帶進度列的功能：

```bash
docker save <映像檔名> | bzip2 | pv | ssh <使用者名稱>@<主電腦名稱> 'cat | docker load'
```
