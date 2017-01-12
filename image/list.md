## 列出映像檔

要想列出已經下載下來的映像檔，可以使用 `docker images` 指令。

```bash
$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
redis                latest              5f515359c7f8        5 days ago          183 MB
nginx                latest              05a60462f8ba        5 days ago          181 MB
mongo                3.2                 fe9198c04d62        5 days ago          342 MB
<none>               <none>              00285df0df87        5 days ago          342 MB
ubuntu               16.04               f753707788c5        4 weeks ago         127 MB
ubuntu               latest              f753707788c5        4 weeks ago         127 MB
ubuntu               14.04               1e0c3dd64ccd        4 weeks ago         188 MB
```

清單包含了倉庫名、標籤、映像檔 ID、建立日期以及所佔用的空間。

其中倉庫名、標籤在之前的基礎概念章節已經介紹過了。**映像檔 ID** 則是映像檔的唯一識別，一個映像檔可以對應多個**標籤**。因此，在上面的例子中，我們可以看到 `ubuntu:16.04` 和 `ubuntu:latest` 擁有相同的 ID，因為它們對應的是同一個映像檔。

### 映像檔體積

如果仔細觀察，會注意到，這裡識別的所佔用空間和在 Docker Hub 上看到的映像檔大小不同。比如，`ubuntu:16.04` 映像檔大小，在這裡是 `127 MB`，但是在 [Docker Hub](https://hub.docker.com/r/library/ubuntu/tags/) 顯示的卻是 `50 MB`。這是因為 Docker Hub 中顯示的體積是壓縮後的體積。在映像檔下載和上傳程序中映像檔是保持著壓縮狀態的，因此 Docker Hub 所顯示的大小是網路傳輸中更關心的流量大小。而 `docker images` 顯示的是映像檔下載到本地後，展開的大小，準確說，是展開後的各層所佔空間的總和，因為映像檔到本地後， 檢視空間的時候，更關心的是本地磁碟空間佔用的大小。

另外一個需要注意的問題是，`docker images` 清單中的映像檔體積總和並非是所有映像檔實際硬碟消耗。由於 Docker 映像檔是多層儲存結構，並且可以繼承、復用，因此不同映像檔可能會因為使用相同的基礎映像檔，從而擁有共同的層。由於 Docker 使用 Union FS，相同的層只需要儲存一份即可，因此實際映像檔硬碟佔用空間很可能要比這個清單映像檔大小的總和要小的多。

### 虛懸映像檔

上面的映像檔清單中，還可以看到一個特殊的映像檔，這個映像檔既沒有倉庫名，也沒有標籤，均為 `<none>`。：

```bash
<none>               <none>              00285df0df87        5 days ago          342 MB
```

這個映像檔原本是有映像檔名和標籤的，原來為 `mongo:3.2`，隨著官方映像檔維護，發佈了新版本後，重新 `docker pull mongo:3.2` 時，`mongo:3.2` 這個映像檔名被轉移到了新下載的映像檔身上，而舊的映像檔上的這個名稱則被取消，從而成為了 `<none>`。除了 `docker pull` 可能導致這種情況，`docker build` 也同樣可以導致這種現象。由於新舊映像檔同名，舊映像檔名稱被取消，從而出現倉庫名、標籤均為 `<none>` 的映像檔。這類無標籤映像檔也被稱為 **虛懸映像檔(dangling image)** ，可以用下面的指令專門顯示這類映像檔：

```bash
$ docker images -f dangling=true
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              00285df0df87        5 days ago          342 MB
```

一般來說，虛懸映像檔已經失去了存在的價值，是可以隨意刪除的，可以用下面的指令刪除。

```bash
$ docker rmi $(docker images -q -f dangling=true)
```

### 中間層映像檔

為了加速映像檔建構、重複利用資源，Docker 會利用 **中間層映像檔**。所以在使用一段時間後，可能會看到一些依賴的中間層映像檔。 預設的 `docker images` 清單中只會顯示頂層映像檔，如果希望顯示包括中間層映像檔在內的所有映像檔的話，需要加 `-a` 參數。

```bash
$ docker images -a
```

這樣會看到很多無標籤的映像檔，與之前的虛懸映像檔不同，這些無標籤的映像檔很多都是中間層映像檔，是其它映像檔所依賴的映像檔。這些無標籤映像檔不應該刪除，否則會導致上層映像檔因為依賴丟失而出錯。實際上，這些映像檔也沒必要刪除，因為之前說過，相同的層只會存一遍，而這些映像檔是別的映像檔的依賴，因此並不會因為它們被列出來而多存了一份，無論如何你也會需要它們。只要刪除那些依賴它們的映像檔後，這些依賴的中間層映像檔也會被連帶刪除。

### 列出部分映像檔

不加任何參數的情況下，`docker images` 會列出所有頂級映像檔，但是有時候我們只希望列出部分映像檔。`docker images` 有好幾個參數可以說明做到這個事情。

根據倉庫名列出映像檔

```bash
$ docker images ubuntu
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              16.04               f753707788c5        4 weeks ago         127 MB
ubuntu              latest              f753707788c5        4 weeks ago         127 MB
ubuntu              14.04               1e0c3dd64ccd        4 weeks ago         188 MB
```

列出特定的某個映像檔，也就是說指定倉庫名和標籤

```bash
$ docker images ubuntu:16.04
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              16.04               f753707788c5        4 weeks ago         127 MB
```

除此以外，`docker images` 還支援強大的篩選器參數 `--filter`，或是簡寫 `-f`。之前我們已經看到了使用篩選器來列出虛懸映像檔的使用方式，它還有更多的使用方式。比如，我們希望看到在 `mongo:3.2` 之後建立的映像檔，可以用下面的指令：

```bash
$ docker images -f since=mongo:3.2
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              5f515359c7f8        5 days ago          183 MB
nginx               latest              05a60462f8ba        5 days ago          181 MB
```

想 檢視某個位置之前的映像檔也可以，只需要把 `since` 換成 `before` 即可。

此外，如果映像檔建構時，定義了 `LABEL`，還可以透過 `LABEL` 來過濾。

```bash
$ docker images -f label=com.example.version=0.1
...
```

### 以特定格式顯示

 預設情況下，`docker images` 會輸出一個完整的表格，但是我們並非所有時候都會需要這些內容。比如，剛才刪除虛懸映像檔的時候，我們需要利用 `docker images` 把所有的虛懸映像檔的 ID 列出來，然後才可以交給 `docker rmi` 指令作為參數來刪除指定的這些映像檔，這個時候就用到了 `-q` 參數。

```bash
$ docker images -q
5f515359c7f8
05a60462f8ba
fe9198c04d62
00285df0df87
f753707788c5
f753707788c5
1e0c3dd64ccd
```

`--filter` 配合 `-q` 產生出指定範圍的 ID 清單，然後送給另一個 `docker` 指令作為參數，從而針對這組實體成批的進行某種作業的做法在 Docker 指令行指令列使用程序中非常常見，不僅僅是映像檔，將來我們會在各個指令中看到這類搭配以完成很強大的功能。因此每次在文件看到篩選器後，可以多注意一下它們的使用方式。

另外一些時候，我們可能只是對表格的結構不滿意，希望自己組織列；或是不希望有標題，這樣方便其它程式解析結果等，這就用到了 [Go 的 範本語法](https://gohugo.io/templates/go-templates/)。

比如，下面的指令會直接列出映像檔結果，並且只包含映像檔ID和倉庫名：

```bash
$ docker images --format "{{.ID}}: {{.Repository}}"
5f515359c7f8: redis
05a60462f8ba: nginx
fe9198c04d62: mongo
00285df0df87: <none>
f753707788c5: ubuntu
f753707788c5: ubuntu
1e0c3dd64ccd: ubuntu
```

或是打算以表格等距顯示，並且有標題行，和 預設一樣，不過自己定義列：

```bash
$ docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
IMAGE ID            REPOSITORY          TAG
5f515359c7f8        redis               latest
05a60462f8ba        nginx               latest
fe9198c04d62        mongo               3.2
00285df0df87        <none>              <none>
f753707788c5        ubuntu              16.04
f753707788c5        ubuntu              latest
1e0c3dd64ccd        ubuntu              14.04
```
