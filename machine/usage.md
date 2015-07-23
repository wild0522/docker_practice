## 使用

Docker Machine 支持多種後端驅動，包括虛擬機、本地主機和雲平台等。

### 本地主機實例
首先確保本地主機可以通過 user 賬號的 key 直接 ssh 到目標主機。

使用 generic 類型的驅動，創建一台 Docker 主機，命名為 test。
```sh
$ docker-machine create -d generic --generic-ip-address=10.0.100.101 --generic-ssh-user=user test
```

創建主機成功後，可以通過 env 命令來讓後續操作對象都是目標主機。
```sh
$ docker-machine env test
```

### 支持驅動
通過 `-d` 選項可以選擇支持的驅動類型。
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


### 操作命令
* `active`                查看活躍的 Docker 主機
* `config`                輸出連接的配置信息
* `create`                創建一個 Docker 主機
* `env`                   顯示連接到某個主機需要的環境變量
* `inspect`               輸出主機更多信息
* `ip`                    獲取主機地址
* `kill`                  停止某個主機
* `ls`                    列出所有管理的主機
* `regenerate-certs`      為某個主機重新生成 TLS 認證信息
* `restart`               重啟主機
* `rm`                    刪除某台主機
* `ssh`                   SSH 到主機上執行命令
* `scp`                   在主機之間複製文件
* `start`                 啟動一個主機
* `stop`                  停止一個主機
* `upgrade`               更新主機 Docker 版本為最新
* `url`                   獲取主機的 URL
* `help, h`               輸出幫助信息

每個命令，又帶有不同的參數，可以通過
```sh
docker-machine <COMMAND> -h
```
來查看具體的用法。
