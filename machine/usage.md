## 使用

Docker Machine 支援多種後端驅動，包括虛擬機、本地主電腦和雲平台等。

### 本地主電腦實例
首先確保本地主電腦可以透過 user 賬號的 key 直接 ssh 到目標主電腦。

使用 generic 類型的驅動，建立一台 Docker 主電腦，命名為 test。
```bash
$ docker-machine create -d generic --generic-ip-address=10.0.100.101 --generic-ssh-user=user test
```

建立主電腦成功後，可以透過 env 指令來讓後續作業對象都是目標主電腦。
```bash
$ docker-machine env test
```

### 支援驅動
透過 `-d` 選項可以選擇支援的驅動類型。
* amazonec2
* azure
* digitalocean
* exoscale
* generic
* google
* none
* openstack
* rackspace
* softlayer
* virtualbox
* vmwarevcloudair
* vmwarevsphere


### 作業指令
* `active`                 檢視活躍的 Docker 主電腦
* `config`                輸出連線的設定資訊
* `create`                建立一個 Docker 主電腦
* `env`                   顯示連線到某個主電腦需要的環境變數
* `inspect`               輸出主電腦更多資訊
* `ip`                    取得主電腦位址
* `kill`                  停止某個主電腦
* `ls`                    列出所有管理的主電腦
* `regenerate-certs`      為某個主電腦重新產生 TLS 認證資訊
* `restart`               重啟主電腦
* `rm`                    刪除某台主電腦
* `ssh`                   SSH 到主電腦上執行指令
* `scp`                   在主電腦之間複製檔案
* `start`                 啟動一個主電腦
* `stop`                  停止一個主電腦
* `upgrade`               更新主電腦 Docker 版本為最新
* `url`                   取得主電腦的 URL
* `help, h`               輸出說明資訊

每個指令，又帶有不同的參數，可以透過
```bash
docker-machine <COMMAND> -h
```
來 檢視具體的使用方式。
