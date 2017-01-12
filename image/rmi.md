## 刪除本地映像檔

如果要刪除本地的映像檔，可以使用 `docker rmi` 指令，其格式為：

```bash
docker rmi [選項] <映像檔1> [<映像檔2> ...]
```

*注意 `docker rm` 指令是刪除容器，不要混淆。*

### 用 ID、映像檔名、摘要刪除映像檔

其中，`<映像檔>` 可以是 `映像檔短 ID`、`映像檔長 ID`、`映像檔名` 或是 `映像檔摘要`。

比如我們有這麼一些映像檔：

```bash
$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
centos                      latest              0584b3d2cf6d        3 weeks ago         196.5 MB
redis                       alpine              501ad78535f0        3 weeks ago         21.03 MB
docker                      latest              cf693ec9b5c7        3 weeks ago         105.1 MB
nginx                       latest              e43d811ce2f4        5 weeks ago         181.5 MB
```

我們可以用映像檔的完整 ID，也稱為 `長 ID`，來刪除映像檔。使用指令碼的時候可能會用長 ID，但是人工輸入就太累了，所以更多的時候是用 `短 ID` 來刪除映像檔。`docker images`  預設列出的就已經是短 ID 了，一般取前3個字型以上，只要足夠區分於別的映像檔就可以了。

比如這裡，如果我們要刪除 `redis:alpine` 映像檔，可以執行：

```bash
$ docker rmi 501
Untagged: redis:alpine
Untagged: redis@sha256:f1ed3708f538b537eb9c2a7dd50dc90a706f7debd7e1196c9264edeea521a86d
Deleted: sha256:501ad78535f015d88872e13fa87a828425117e3d28075d0c117932b05bf189b7
Deleted: sha256:96167737e29ca8e9d74982ef2a0dda76ed7b430da55e321c071f0dbff8c2899b
Deleted: sha256:32770d1dcf835f192cafd6b9263b7b597a1778a403a109e2cc2ee866f74adf23
Deleted: sha256:127227698ad74a5846ff5153475e03439d96d4b1c7f2a449c7a826ef74a2d2fa
Deleted: sha256:1333ecc582459bac54e1437335c0816bc17634e131ea0cc48daa27d32c75eab3
Deleted: sha256:4fc455b921edf9c4aea207c51ab39b10b06540c8b4825ba57b3feed1668fa7c7
```

我們也可以用`映像檔名`，也就是 `<倉庫名>:<標籤>`，來刪除映像檔。

```bash
$ docker rmi centos
Untagged: centos:latest
Untagged: centos@sha256:b2f9d1c0ff5f87a4743104d099a3d561002ac500db1b9bfa02a783a46e0d366c
Deleted: sha256:0584b3d2cf6d235ee310cf14b54667d889887b838d3f3d3033acd70fc3c48b8a
Deleted: sha256:97ca462ad9eeae25941546209454496e1d66749d53dfa2ee32bf1faabd239d38
```

當然，更精確的是使用 `映像檔摘要` 刪除映像檔。

```bash
$ docker images --digests
REPOSITORY                  TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
node                        slim                sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228   6e0c4c8e3913        3 weeks ago         214 MB

$ docker rmi node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228
Untagged: node@sha256:b4f0e0bdeb578043c1ea6862f0d40cc4afe32a4a582f3be235a3b164422be228
```

### Untagged 和 Deleted

如果觀察上面這幾個指令的執行輸出資訊的話，你會注意到刪除行為分為兩類，一類是 `Untagged`，另一類是 `Deleted`。我們之前介紹過，映像檔的唯一識別是其 ID 和摘要，而一個映像檔可以有多個標籤。

因此當我們使用上面指令刪除映像檔的時候，實際上是在要求刪除某個標籤的映像檔。所以首先需要做的是將滿足我們要求的所有映像檔標籤都取消，這就是我們看到的 `Untagged` 的資訊。因為一個映像檔可以對應多個標籤，因此當我們刪除了所指定的標籤後，可能還有別的標籤指向了這個映像檔，如果是這種情況，那麼 `Delete` 行為就不會發生。所以並非所有的 `docker rmi` 都會產生刪除映像檔的行為，有可能僅僅是取消了某個標籤而已。

當該映像檔所有的標籤都被取消了，該映像檔很可能會失去了存在的意義，因此會觸發刪除行為。映像檔是多層儲存結構，因此在刪除的時候也是從上層向基礎層方向依次進行判斷刪除。映像檔的多層結構讓映像檔復用變動非常容易，因此很有可能某個其它映像檔正依賴於目前映像檔的某一層。這種情況，依舊不會觸發刪除該層的行為。直到沒有任何層依賴目前層時，才會真實的刪除目前層。這就是為什麼，有時候會奇怪，為什麼明明沒有別的標籤指向這個映像檔，但是它還是存在的原因，也是為什麼有時候會發現所刪除的層數和自己 `docker pull` 看到的層數不一樣的源。

除了映像檔依賴以外，還需要注意的是容器對映像檔的依賴。如果有用這個映像檔啟動的容器存在（即使容器沒有執行），那麼同樣不可以刪除這個映像檔。之前講過，容器是以映像檔為基礎，再加一層容器儲存層，組成這樣的多層儲存結構去執行的。因此該映像檔如果被這個容器所依賴的，那麼刪除必然會導致故障。如果這些容器是不需要的，應該先將它們刪除，然後再來刪除映像檔。

### 用 docker images 指令來配合

像其它可以承接多個實體的指令一樣，可以使用 `docker images -q` 來配合使用 `docker rmi`，這樣可以成批的刪除希望刪除的映像檔。比如之前我們介紹過的，刪除虛懸映像檔的指令是：

```bash
$ docker rmi $(docker images -q -f dangling=true)
```

我們在「映像檔清單」章節介紹過很多過濾映像檔清單的方式都可以拿過來使用。

比如，我們需要刪除所有倉庫名為 `redis` 的映像檔：

```bash
$ docker rmi $(docker images -q redis)
```

或是刪除所有在 `mongo:3.2` 之前的映像檔：

```bash
$ docker rmi $(docker images -q -f before=mongo:3.2)
```

充分利用你的想像力和 Linux 指令行指令列的強大，你可以完成很多非常讚的功能。

### CentOS/RHEL 的使用者需要注意的事項

在 Ubuntu/Debian 上有 `UnionFS` 可以使用，如 `aufs` 或是 `overlay2`，而 CentOS 和 RHEL 的核心中沒有相關驅動。因此對於這類系統，一般使用 `devicemapper` 驅動利用 LVM 的一些機制來類比分層儲存。這樣的做法除了效能比較差外，穩定性一般也不好，而且設定相對複雜。Docker 安裝在 CentOS/RHEL 上後，會 預設選擇 `devicemapper`，但是為了簡化設定，其 `devicemapper` 是跑在一個稀疏檔案類比的塊裝置上，也被稱為 `loop-lvm`。這樣的選擇是因為不需要額外設定就可以執行 Docker，這是自動設定唯一能做到的事情。但是 `loop-lvm` 的做法非常不好，其穩定性、效能更差，無論是日誌還是 `docker info` 中都會看到警告資訊。官方文件有明確的文章講解了如何設定塊裝置給 `devicemapper` 驅動做儲存層的做法，這類做法也被稱為設定 `direct-lvm`。

除了前面說到的問題外，`devicemapper` + `loop-lvm` 還有一個缺陷，因為它是稀疏檔案，所以它會不斷增長。使用者在使用程序中會注意到 `/var/lib/docker/devicemapper/devicemapper/data` 不斷增長，而且無法控制。很多人會希望刪除映像檔或是可以解決這個問題，結果發現效果並不明顯。原因就是這個稀疏檔案的空間釋放後基本不進行垃圾回收的問題。因此往往會出現即使刪除了檔案內容，空間卻無法回收，隨著使用這個稀疏檔案一直在不斷增長。

所以對於 CentOS/RHEL 的使用者來說，在沒有辦法使用 `UnionFS` 的情況下，一定要設定 `direct-lvm` 給 `devicemapper`，無論是為了效能、穩定性還是空間利用率。

*或許有人注意到了 CentOS 7 中存在被 backports 回來的 `overlay` 驅動，不過 CentOS 裡的這個驅動達不到生產環境使用的穩定程度，所以不推薦使用。*
