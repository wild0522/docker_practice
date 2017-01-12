## Ubuntu、Debian 系列安裝 Docker

### 系統要求

Docker 支援以下版本的 [Ubuntu](https://www.ubuntu.com/server) 和 [Debian](https://www.debian.org/intro/about) 作業系統：

* Ubuntu Xenial 16.04 (LTS)
* Ubuntu Trusty 14.04 (LTS)
* Ubuntu Precise 12.04 (LTS)
* Debian testing stretch (64-bit)
* Debian 8 Jessie (64-bit)
* Debian 7 Wheezy (64-bit)（*必須啟動 backports*)

Ubuntu 發行版中，LTS（Long-Term-Support）長期支援版本，會獲得 5 年的升級維護支援，這樣的版本會更穩定，因此在生產環境中推薦使用 LTS 版本。

Docker 目前支援的 Ubuntu 版本最低為 12.04 LTS，但從穩定性上考慮，推薦使用 14.04 LTS 或更高的版本。

Docker 需要安裝在 64 位的 x86 平台或 ARM 平台上（如[樹莓派](https://www.raspberrypi.org/)），並且要求核心版本不低於 3.10。但實際上核心越新越好，過低的核心版本可能會出現部分功能無法使用，或是不穩定。

使用者可以透過如下指令檢查自己的核心版本詳細資訊：

```bash
$ uname -a
Linux device 4.4.0-45-generic #66~14.04.1-Ubuntu SMP Wed Oct 19 15:05:38 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

#### 升級核心

如果核心版本過低，可以用下面的指令升級系統核心。

##### Ubuntu 12.04 LTS

```bash
sudo apt-get install -y --install-recommends linux-generic-lts-trusty
```

##### Ubuntu 14.04 LTS

```bash
sudo apt-get install -y --install-recommends linux-generic-lts-xenial
```

##### Debian 7 Wheezy

Debian 7 的核心 預設為 3.2，為了滿足 Docker 的需求，應該安裝 `backports` 的核心。

執行下面的指令加入 `backports` 源：

```bash
$ echo "deb http://http.debian.net/debian wheezy-backports main" | sudo tee /etc/apt/sources.list.d/backports.list
```

升級到 `backports` 核心：

```bash
$ sudo apt-get update
$ sudo apt-get -t wheezy-backports install linux-image-amd64
```

##### Debian 8 Jessie

Debian 8 的核心 預設為 3.16，滿足基本的 Docker 執行條件。但是如果打算使用 `overlay2` 儲存層驅動，或某些功能不夠穩定希望升級到較新版本的核心，可以加入 `backports` 源，升級到新版本的核心。

執行下面的指令加入 `backports` 源：

```bash
$ echo "deb http://http.debian.net/debian jessie-backports main" | sudo tee /etc/apt/sources.list.d/backports.list
```

升級到 `backports` 核心：

```bash
$ sudo apt-get update
$ sudo apt-get -t jessie-backports install linux-image-amd64
```

需要注意的是，升級到 `backports` 的核心之後，會因為 `AUFS` 核心模組無法使用，而使用 預設的 `devicemapper` 驅動，並且設定為 `loop-lvm`，這是不推薦的。因此，不要忘記安裝 Docker 後，設定 `overlay2` 儲存層驅動。

##### 設定 GRUB 引導參數

在 Docker 使用工作階段會議，或是在 `docker info` 資訊中，可能會看到下面的警告資訊：

```bash
WARNING: Your kernel does not support cgroup swap limit. WARNING: Your
kernel does not support swap limit capabilities. Limitation discarded.
```

或是

```bash
WARNING: No memory limit support
WARNING: No swap limit support
WARNING: No oom kill disable support
```

如果需要這些功能，就需要修改 GRUB 的設定檔 ` /etc/default/grub`，在 `GRUB_CMDLINE_LINUX` 中加入核心引導參數 `cgroup_enable=memory swapaccount=1`。

然後不要忘記了更新 GRUB：

```bash
$ sudo update-grub
$ sudo reboot
```

### 使用指令碼自動安裝

Docker 官方為了簡化安裝流程，提供了一套安裝指令碼，Ubuntu 和 Debian 系統可以使用這套指令碼安裝：

```bash
curl -sSL https://get.docker.com/ | sh
```

執行這個指令後，指令碼就會自動的將一切準備工作做好，並且把 Docker 安裝在系統中。

不過，由於偉大的牆的原因，在國內使用這個指令碼可能會出現某些下載出現錯誤的情況。國內的一些雲服務商提供了這個指令碼的修改版本，使其使用國內的 Docker 軟體源映像檔安裝，這樣就避免了牆的干擾。

#### 阿里雲的安裝指令碼

```bash
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```

#### DaoCloud 的安裝指令碼

```bash
curl -sSL https://get.daocloud.io/docker | sh
```

### 手動安裝

#### 安裝所需的套裝軟體

##### 可選核心模組

從 Ubuntu 14.04 開始，一部分核心模組移到了可選核心模組包(`linux-image-extra-*`)，以減少核心套裝軟體的體積。正常安裝的系統應該會包含可選核心模組包，而一些裁剪後的系統可能會將其精簡掉。`AUFS` 核心驅動屬於可選核心模組的一部分，作為推薦的 Docker 儲存層驅動，一般建議安裝可選核心模組包以使用 `AUFS`。

如果系統沒有安裝可選核心模組的話，可以執行下面的指令來安裝可選核心模組包：

```bash
$ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
```

##### 12.04 LTS 圖形界面

在 Ubuntu 12.04 桌面環境下，需要一些額外的套裝軟體，可以用下面的指令安裝。

```bash
$ sudo apt-get install xserver-xorg-lts-trusty libgl1-mesa-glx-lts-trusty
```

####  加入 APT 映像檔源

雖然 Ubuntu 系統軟體源中有 Docker，名為 `docker.io`，但是不應該使用系統源中的這個版本，它的版本太舊。我們需要使用 Docker 官方提供的軟體源，因此，我們需要加入 APT 軟體源。

由於官方源使用 HTTPS 以確保軟體下載程序中不被篡改。因此，我們首先需要加入使用 HTTPS 傳輸的套裝軟體以及 CA 證書。

*國內的一些軟體源映像檔（比如[阿里雲](http://mirrors.aliyun.com/docker-engine/)）不是太在意系統安全上的細節，可能依舊使用不安全的 HTTP，對於這些源可以不執行這一步。*

```bash
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
```

為了確認所下載套裝軟體的合法性，需要加入 Docker 官方軟體源的 GPG 密鑰。

```bash
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

然後，我們需要向 `source.list` 中加入 Docker 軟體源，下表列出了不同的 Ubuntu 和 Debian 版本對應的 APT 源。

|     作業系統版本      |                            REPO                              |
|---------------------|--------------------------------------------------------------|
| Precise 12.04 (LTS) | `deb https://apt.dockerproject.org/repo ubuntu-precise main` |
| Trusty 14.04 (LTS)  | `deb https://apt.dockerproject.org/repo ubuntu-trusty main`  |
| Xenial 16.04 (LTS)  | `deb https://apt.dockerproject.org/repo ubuntu-xenial main`  |
|   Debian 7 Wheezy   | `deb https://apt.dockerproject.org/repo debian-wheezy main`  |
|   Debian 8 Jessie   | `deb https://apt.dockerproject.org/repo debian-jessie main`  |
| Debian Stretch/Sid  | `deb https://apt.dockerproject.org/repo debian-stretch main` |

用下面的指令將 APT 源新增至 `source.list`（將其中的 `<REPO>` 取代為上表的值）：

```bash
$ echo "<REPO>" | sudo tee /etc/apt/sources.list.d/docker.list
```

加入成功後，更新 apt 套裝軟體快取。

```bash
$ sudo apt-get update
```

#### 安裝 Docker

在一切準備就緒後，就可以安裝最新版本的 Docker 了，套裝軟體名稱為 `docker-engine`。

```bash
$ sudo apt-get install docker-engine
```

如果系統中存在舊版本的 Docker （`lxc-docker`, `docker.io`），會提示是否先刪除，選擇是即可。

#### 啟動 Docker 引擎

##### Ubuntu 12.04/14.04、Debian 7 Wheezy

```bash
$ sudo service docker start
```

##### Ubuntu 16.04、Debian 8 Jessie/Stretch

```bash
$ sudo systemctl enable docker
$ sudo systemctl start docker
```

#### 建立 docker 使用者組

 預設情況下，`docker` 指令會使用 [Unix socket](https://en.wikipedia.org/wiki/Unix_domain_socket) 與 Docker 引擎通訊。而只有 `root` 使用者和 `docker` 組的使用者才可以存取 Docker 引擎的 Unix socket。出於安全考慮，一般 Linux 系統上不會直接使用 `root` 使用者。因此，更好地做法是將需要使用 `docker` 的使用者加入 `docker` 使用者組。

建立 `docker` 組：

```bash
$ sudo groupadd docker
```

將目前使用者加入 `docker` 組：

```bash
$ sudo usermod -aG docker $USER
```

### 參考文件

* [Docker 官方 Ubuntu 安裝文件](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
* [Docker 官方 Debian 安裝文件](https://docs.docker.com/engine/installation/linux/debian/)
