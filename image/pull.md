## 取得映像檔

之前提到過，[Docker Hub](https://hub.docker.com/explore/) 上有大量的高品質的映像檔可以用，這裡我們就說一下怎麼取得這些映像檔並執行。

從 Docker Registry 取得映像檔的指令是 `docker pull`。其指令格式為：

```bash
docker pull [選項] [Docker Registry位址]<倉庫名>:<標籤>
```

具體的選項可以透過 `docker pull --help` 指令看到，這裡我們說一下映像檔名稱的格式。

* Docker Registry位址：位址的格式一般是 `<網域名/IP>[:連接埠號碼]`。 預設位址是 Docker Hub。
* 倉庫名：如之前所說，這裡的倉庫名是兩段式名稱，既 `<使用者名稱>/<軟體名>`。對於 Docker Hub，如果不給出使用者名稱，則 預設為 `library`，也就是官方映像檔。

比如：

```bash
$ docker pull ubuntu:14.04
14.04: Pulling from library/ubuntu
bf5d46315322: Pull complete
9f13e0ac480c: Pull complete
e8988b5b3097: Pull complete
40af181810e7: Pull complete
e6f7c7e5c03e: Pull complete
Digest: sha256:147913621d9cdea08853f6ba9116c2e27a3ceffecf3b492983ae97c3d643fbbe
Status: Downloaded newer image for ubuntu:14.04
```

上面的指令中沒有給出 Docker Registry 位址，因此將會從 Docker Hub 取得映像檔。而映像檔名稱是 `ubuntu:14.04`，因此將會取得官方映像檔 `library/ubuntu` 倉庫中標籤為 `14.04` 的映像檔。

從下載程序中可以看到我們之前提及的分層儲存的概念，映像檔是由多層儲存所構成。下載也是一層層的去下載，並非單一檔案。下載程序中給出了每一層的 ID 的前 12 位。並且下載結束後，給出該映像檔完整的 `sha256` 的摘要，以確保下載一致性。

在實驗上面指令的時候，你可能會發現，你所看到的層 ID 以及 `sha256` 的摘要和這裡的不一樣。這是因為官方映像檔是一直在維護的，有任何新的 bug，或是版本更新，都會進行修復再以原來的標籤發佈，這樣可以確保任何使用這個標籤的使用者可以獲得更安全、更穩定的映像檔。

*如果從 Docker Hub 下載映像檔非常緩慢，可以參照後面的章節設定加速器。*

### 執行

有了映像檔後，我們就可以以這個映像檔為基礎啟動一個容器來執行。以上面的 `ubuntu:14.04` 為例，如果我們打算啟動裡面的 `bash` 並且進行交談式作業的話，可以執行下面的指令。

```bash
$ docker run -it --rm ubuntu:14.04 bash
root@e7009c6ce357:/# cat /etc/os-release
NAME="Ubuntu"
VERSION="14.04.5 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.5 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
root@e7009c6ce357:/# exit
exit
$
```

`docker run` 就是執行容器的指令，具體格式我們會在後面的章節講解，我們這裡簡要的說明一下上面用到的參數。

* `-it`：這是兩個參數，一個是 `-i`：交談式作業，一個是 `-t` 終端機。我們這裡打算進入 `bash` 執行一些指令並 檢視傳回結果，因此我們需要交談式終端機。
* `--rm`：這個參數是說容器結束後隨之將其刪除。 預設情況下，為了排障需求，結束的容器並不會立即刪除，除非手動 `docker rm`。我們這裡只是隨便執行個指令，看看結果，不需要排障和保留結果，因此使用 `--rm` 可以避免浪費空間。
* `ubuntu:14.04`：這是指用 `ubuntu:14.04` 映像檔為基礎來啟動容器。
* `bash`：放在映像檔名後的是**指令**，這裡我們希望有個交談式 Shell，因此用的是 `bash`。

進入容器後，我們可以在 Shell 下作業，執行任何所需的指令。這裡，我們執行了 `cat /etc/os-release`，這是 Linux 常用的 檢視目前系統版本的指令，從傳回的結果可以看到容器內是 `Ubuntu 14.04.5 LTS` 系統。

最後我們透過 `exit` 結束了這個容器。
