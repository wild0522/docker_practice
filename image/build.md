## 使用 Dockerfile 訂製映像檔

從剛才的 `docker commit` 的學習中，我們可以瞭解到，映像檔的訂製實際上就是訂製每一層所加入的設定、檔案。如果我們可以把每一層修改、安裝、建構、作業的指令都寫入一個指令碼，用這個指令碼來建構、訂製映像檔，那麼之前提及的無法重複的問題、映像檔建構透明性的問題、體積的問題就都會解決。這個指令碼就是 Dockerfile。

Dockerfile 是一個文字檔案，其內包含了一條條的**指令(Instruction)**，每一條指令建構一層，因此每一條指令的內容，就是描述該層應當如何建構。

還以之前訂製 `nginx` 映像檔為例，這次我們使用 Dockerfile 來訂製。

在一個空白目錄中，建立一個文字檔案，並命名為 `Dockerfile`：

```bash
$ mkdir mynginx
$ cd mynginx
$ touch Dockerfile
```

其內容為：

```Dockerfile
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

這個 Dockerfile 很簡單，一共就兩行。涉及到了兩條指令，`FROM` 和 `RUN`。

### FROM 指定基礎映像檔

所謂訂製映像檔，那一定是以一個映像檔為基礎，在其上進行訂製。就像我們之前執行了一個 `nginx` 映像檔的容器，再進行修改一樣，基礎映像檔是必須指定的。而 `FROM` 就是指定**基礎映像檔**，因此一個 `Dockerfile` 中 `FROM` 是必備的指令，並且必須是第一筆指令。

在 Docker Hub (<https://hub.docker.com/explore/>) 上有非常多的高品質的官方映像檔，
有可以直接拿來使用的服務類的映像檔，如 [`nginx`](https://hub.docker.com/_/nginx/)、[`redis`](https://hub.docker.com/_/redis/)、[`mongo`](https://hub.docker.com/_/mongo/)、[`mysql`](https://hub.docker.com/_/mongo/)、[`httpd`](https://hub.docker.com/_/httpd/)、[`php`](https://hub.docker.com/_/php/)、[`tomcat`](https://hub.docker.com/_/tomcat/) 等；
也有一些方便開發、建構、執行各種語言應用的映像檔，如 [`node`](https://hub.docker.com/_/node/)、[`openjdk`](https://hub.docker.com/_/openjdk/)、[`python`](https://hub.docker.com/_/python/)、[`ruby`](https://hub.docker.com/_/ruby/)、[`golang`](https://hub.docker.com/_/golang/) 等。
可以在其中尋找一個最符合我們最終目標的映像檔為基礎映像檔進行訂製。
如果沒有找到對應服務的映像檔，官方映像檔中還提供了一些更為基礎的作業系統映像檔，如 [`ubuntu`](https://hub.docker.com/_/ubuntu/)、[`debian`](https://hub.docker.com/_/debian/)、[`centos`](https://hub.docker.com/_/centos/)、[`fedora`](https://hub.docker.com/_/fedora/)、[`alpine`](https://hub.docker.com/_/alpine/) 等，這些作業系統的軟體庫為我們提供了更廣闊的延伸空間。

除了選擇現有映像檔為基礎映像檔外，Docker 還存在一個特殊的映像檔，名為 `scratch`。這個映像檔是虛擬的概念，並不實際存在，它表示一個空白的映像檔。

```Dockerfile
FROM scratch
...
```

如果你以 `scratch` 為基礎映像檔的話，意味著你不以任何映像檔為基礎，接下來所寫的指令將作為映像檔第一層開始存在。

不以任何系統為基礎，直接將可執行檔案複製進映像檔的做法並不罕見，比如 [`swarm`](https://hub.docker.com/_/swarm/)、[`coreos/etcd`](https://quay.io/repository/coreos/etcd)。對於 Linux 下靜態編譯的程式來說，並不需要有作業系統提供執行時支援，所需的一切庫都已經在可執行檔案裡了，因此直接 `FROM scratch` 會讓映像檔體積更加小巧。使用 [Go 語言](https://golang.org/) 開發的應用很多會使用這種方式來製作映像檔，這也是為什麼有人認為 Go 是特別適合容器微服務架構的語言的原因之一。

### RUN 執行指令

`RUN` 指令是用來執行指令行指令列指令的。由於指令行指令列的強大能力，`RUN` 指令在訂製映像檔時是最常用的指令之一。其格式有兩種：

* *shell* 格式：`RUN <指令>`，就像直接在指令行指令列中輸入的指令一樣。剛才寫的 Dockrfile 中的 `RUN` 指令就是這種格式。

```Dockerfile
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

* *exec* 格式：`RUN ["可執行檔案", "參數1", "參數2"]`，這更像是函數叫用中的格式。

既然 `RUN` 就像 Shell 指令碼一樣可以執行指令，那麼我們是否就可以像 Shell 指令碼一樣把每個指令對應一個 RUN 呢？比如這樣：

```Dockerfile
FROM debian:jessie

RUN apt-get update
RUN apt-get install -y gcc libc6-dev make
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz"
RUN mkdir -p /usr/src/redis
RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
RUN make -C /usr/src/redis
RUN make -C /usr/src/redis install
```

之前說過，Dockerfile 中每一個指令都會建立一層，`RUN` 也不例外。每一個 `RUN` 的行為，就和剛才我們手動建立映像檔的程序一樣：新增立一層，在其上執行這些指令，執行結束後，`commit` 這一層的修改，構成新的映像檔。

而上面的這種寫法，建立了 7 層映像檔。這是完全沒有意義的，而且很多執行時不需要的東西，都被裝進了映像檔裡，比如編譯環境、更新的套裝軟體等等。結果就是產生非常臃腫、非常多層的映像檔，不僅僅增加了建構部署的時間，也很容易出錯。
這是很多初學 Docker 的人常犯的一個錯誤。

*Union FS 是有最大層數限制的，比如 AUFS，曾經是最大不得超過 42 層，現在是不得超過 127 層。*

上面的 `Dockerfile` 正確的寫法應該是這樣：

```Dockerfile
FROM debian:jessie

RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
```

首先，之前所有的指令只有一個目的，就是編譯、安裝 redis 可執行檔案。因此沒有必要建立很多層，這只是一層的事情。因此，這裡沒有使用很多個 `RUN` 對一一對應不同的指令，而是僅僅使用一個 `RUN` 指令，並使用 `&&` 將各個所需指令串聯起來。將之前的 7 層，簡化為了 1 層。在撰寫 Dockerfile 的時候，要經常提醒自己，這並不是在寫 Shell 指令碼，而是在定義每一層該如何建構。

並且，這裡為了格式化還進行了自動換行。Dockerfile 支援 Shell 類的行尾加入 `\` 的指令自動換行方式，以及行首 `#` 進行註釋的格式。良好的格式，比如自動換行、縮排、註釋等，會讓維護、排障更為容易，這是一個比較好的習慣。

此外，還可以看到這一組指令的最後加入了清理工作的指令，刪除了為了編譯建構所需要的軟體，清理了所有下載、展開的檔案，並且還清理了 `apt` 快取檔案。這是很重要的一步，我們之前說過，映像檔是多層儲存，每一層的東西並不會在下一層被刪除，會一直跟隨著映像檔。因此映像檔建構時，一定要確保每一層只加入真正需要加入的東西，任何無關的東西都應該清理掉。

很多人初學 Docker 製作出了很臃腫的映像檔的原因之一，就是忘記了每一層建構的最後一定要清理掉無關檔案。

### 建構映像檔

好了，讓我們再回到之前訂製的 nginx 映像檔的 Dockerfile 來。現在我們明白了這個 Dockerfile 的內容，那麼讓我們來建構這個映像檔吧。

在 `Dockerfile` 檔案所在目錄執行：

```bash
$ docker build -t nginx:v3 .
Sending build context to Docker daemon 2.048 kB
Step 1 : FROM nginx
 ---> e43d811ce2f4
Step 2 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
 ---> Running in 9cdc27646c7b
 ---> 44aa4490ce2c
Removing intermediate container 9cdc27646c7b
Successfully built 44aa4490ce2c
```

從指令的輸出結果中，我們可以清晰的看到映像檔的建構程序。在 `Step 2` 中，如同我們之前所說的那樣，`RUN` 指令啟動了一個容器 `9cdc27646c7b`，執行了所要求的指令，並最後送出了這一層 `44aa4490ce2c`，隨後刪除了所用到的這個容器 `9cdc27646c7b`。

這裡我們使用了 `docker build` 指令進行映像檔建構。其格式為：

```bash
docker build [選項] <上下文路徑/URL/->
```

在這裡我們指定了最終映像檔的名稱 `-t nginx:v3`，建構成功後，我們可以像之前執行 `nginx:v2` 那樣來執行這個映像檔，其結果會和 `nginx:v2` 一樣。

### 映像檔建構上下文（Context）

如果注意，會看到 `docker build` 指令最後有一個 `.`。`.` 表示目前目錄，而 `Dockerfile` 就在目前目錄，因此不少初學者以為這個路徑是在指定 `Dockerfile` 所在路徑，這麼理解其實是不準確的。如果對應上面的指令格式，你可能會發現，這是在指定**上下文路徑**。那麼什麼是上下文呢？

首先我們要理解 `docker build` 的工作原理。Docker 在執行時分為 Docker 引擎（也就是服務端守護處理序）和用戶端工具。Docker 的引擎提供了一組 REST API，被稱為 [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api/)，而如 `docker` 指令這樣的用戶端工具，則是透過這組 API 與 Docker 引擎互動，從而完成各種功能。因此，雖然表面上我們好像是在本機執行各種 `docker` 功能，但實際上，一切都是使用的遠端叫用形式在服務端（Docker 引擎）完成。也因為這種 C/S 設計，讓我們作業遠端伺服器的 Docker 引擎變得輕而易舉。

當我們進行映像檔建構的時候，並非所有訂製都會透過 `RUN` 指令完成，經常會需要將一些本地檔案複製進映像檔，比如透過 `COPY` 指令、`ADD` 指令等。而 `docker build` 指令建構映像檔，其實並非在本地建構，而是在服務端，也就是 Docker 引擎中建構的。那麼在這種用戶端/服務端的架構中，如何才能讓服務端獲得本地檔案呢？

這就引入了上下文的概念。當建構的時候，使用者會指定建構映像檔上下文的路徑，`docker build` 指令得知這個路徑後，會將路徑下的所有內容打包，然後上傳給 Docker 引擎。這樣 Docker 引擎收到這個上下文包後，展開就會獲得建構映像檔所需的一切檔案。

如果在 `Dockerfile` 中這麼寫：

```Dockerfile
COPY ./package.json /app/
```

這並不是要複製執行 `docker build` 指令所在的目錄下的 `package.json`，也不是複製 `Dockerfile` 所在目錄下的 `package.json`，而是複製 **上下文（context）** 目錄下的 `package.json`。

因此，`COPY` 這類指令中的原始檔的路徑都是*相對路徑*。這也是初學者經常會問的為什麼 `COPY ../package.json /app` 或是 `COPY /opt/xxxx /app` 無法工作的原因，因為這些路徑已經超出了上下文的範圍，Docker 引擎無法獲得這些位置的檔案。如果真的需要那些檔案，應該將它們複製到上下文目錄中去。

現在就可以理解剛才的指令 `docker build -t nginx:v3 .` 中的這個 `.`，實際上是在指定上下文的目錄，`docker build` 指令會將該目錄下的內容打包交給 Docker 引擎以說明建構映像檔。

如果觀察 `docker build` 輸出，我們其實已經看到了這個傳送上下文的程序：

```bash
$ docker build -t nginx:v3 .
Sending build context to Docker daemon 2.048 kB
...
```

理解建構上下文對於映像檔建構是很重要的，避免犯一些不應該的錯誤。比如有些初學者在發現 `COPY /opt/xxxx /app` 不工作後，於是乾脆將 `Dockerfile` 放到了硬碟根目錄去建構，結果發現 `docker build` 執行後，在傳送一個幾十 GB 的東西，極為緩慢而且很容易建構失敗。那是因為這種做法是在讓 `docker build` 打包整個硬碟，這顯然是使用錯誤。

一般來說，應該會將 `Dockerfile` 至於一個空目錄下，或是項目根目錄下。如果該目錄下沒有所需檔案，那麼應該把所需檔案複製一份過來。如果目錄下有些東西確實不希望建構時傳給 Docker 引擎，那麼可以用 `.gitignore` 一樣的語法寫一個 `.dockerignore`，該檔案是用於剔除不需要作為上下文傳遞給 Docker 引擎的。

那麼為什麼會有人誤以為 `.` 是指定 `Dockerfile` 所在目錄呢？這是因為在 預設情況下，如果不額外指定 `Dockerfile` 的話，會將上下文目錄下的名為 `Dockerfile` 的檔案作為 Dockerfile。

這只是 預設行為，實際上 `Dockerfile` 的檔案名稱並不要求必須為 `Dockerfile`，而且並不要求必須位於上下文目錄中，比如可以用 `-f ../Dockerfile.php` 參數指定某個檔案作為 `Dockerfile`。

當然，一般大家習慣性的會使用 預設的檔案名稱 `Dockerfile`，以及會將其置於映像檔建構上下文目錄中。

### 其它 `docker build` 的使用方式

#### 直接用 Git repo 進行建構

或許你已經注意到了，`docker build` 還支援從 URL 建構，比如可以直接從 Git repo 中建構：

```bash
$ docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14
docker build https://github.com/twang2218/gitlab-ce-zh.git\#:8.14
Sending build context to Docker daemon 2.048 kB
Step 1 : FROM gitlab/gitlab-ce:8.14.0-ce.0
8.14.0-ce.0: Pulling from gitlab/gitlab-ce
aed15891ba52: Already exists
773ae8583d14: Already exists
...
```

這行指令指定了建構所需的 Git repo，並且指定 預設的 `master` 分支，建構目錄為 `/8.14/`，然後 Docker 就會自己去 `git clone` 這個項目、切換到指定分支、並進入到指定目錄後開始建構。

#### 用設定的 tar 壓縮包建構

```bash
$ docker build http://server/context.tar.gz
```

如果所給出的 URL 不是個 Git repo，而是個 `tar` 壓縮包，那麼 Docker 引擎會下載這個包，並自動解壓縮，以其作為上下文，開始建構。

#### 從標準輸入中讀取 Dockerfile 進行建構

```bash
docker build - < Dockerfile
```

或

```bash
cat Dockerfile | docker build -
```

如果標準輸入傳入的是文字檔案，則將其視為 `Dockerfile`，並開始建構。這種形式由於直接從標準輸入中讀取 Dockerfile 的內容，它沒有上下文，因此不可以像其它方法那樣可以將本地檔案 `COPY` 進映像檔之類的事情。

#### 從標準輸入中讀取上下文壓縮包進行建構

```bash
$ docker build - < context.tar.gz
```

如果發現標準輸入的檔案格式是 `gzip`、`bzip2` 以及 `xz` 的話，將會使其為上下文壓縮包，直接將其展開，將裡面視為上下文，並開始建構。
