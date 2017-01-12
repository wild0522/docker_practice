## CentOS 作業系統安裝 Docker

### 系統要求

Docker 最低支援 CentOS 7。

Docker 需要安裝在 64 位的平台，並且核心版本不低於 3.10。 CentOS 7 滿足最低核心的要求，但由於核心版本比較低，部分功能（如 `overlay2` 儲存層驅動）無法使用，並且部分功能可能不太穩定。

### 使用指令碼自動安裝

Docker 官方為了簡化安裝流程，提供了一套安裝指令碼，CentOS 系統上可以使用這套指令碼安裝：

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

#### 加入核心參數

 預設設定下，在 CentOS 使用 Docker 可能會碰到下面的這些警告資訊：

```bash
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```

加入核心設定參數以啟動這些功能。

```bash
$ sudo tee -a /etc/sysctl.conf <<-EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

然後重新載入 `sysctl.conf` 即可

```bash
$ sudo sysctl -p
```

####  加入 yum 源

雖然 CentOS 軟體源 `Extras` 中有 Docker，名為 `docker`，但是不建議使用系統源中的這個版本，它的版本相對比較陳舊，而且並非 Docker 官方維護的版本。因此，我們需要使用 Docker 官方提供的 CentOS 軟體源。

執行下面的指令加入 `yum` 軟體源。

```bash
$ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

#### 安裝 Docker

更新 `yum` 軟體源快取，並安裝 `docker-engine`。

```bash
$ sudo yum update
$ sudo yum install docker-engine
```

#### 啟動 Docker 引擎

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

參見 [Docker 官方 CentOS 安裝文件](https://docs.docker.com/engine/installation/linux/centos/)。
