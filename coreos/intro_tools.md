#CoreOS工具介紹

CoreOS提供了三大工具，它們分別是：服務發現，容器管理和處理序管理。

##使用etcd服務發現

CoreOS的第一個重要元件就是使用etcd來實作的服務發現。

如果你使用 預設的樣例cloud-config檔案，那麼etcd會在啟動時自動執行。

例如：

```
#cloud-config

hostname: coreos0
ssh_authorized_keys:
  - ssh-rsa AAAA...
coreos:
  units:
    - name: etcd.service
      command: start
    - name: fleet.service
      command: start
  etcd:
    name: coreos0
    discovery: https://discovery.etcd.io/<token>
```

設定檔裡有一個token，取得它可以透過如下方式：

存取位址

https://discovery.etcd.io/new

你將會取得一個包含你得teoken得URL。

##透過Docker進行容器管理

第二個元件就是docker，它用來執行你的程式碼和應用。

每一個CoreOS的機器上都安裝了它，具體使用請參考本書其它章節。

##使用fleet進行處理序管理

第三個CoreOS元件是fleet。

它是叢集的分散式起始系統。你應該使用fleet來管理你的docker容器的生命週期。

Fleet透過接受systemd單元檔案來工作，同時在你叢集的機器上透過單元檔案中編輯的偏好來對它們進行調度。

首先，讓我們建構一個簡單的可以執行docker容器的systemd單元。把這個檔案儲存在home目錄並命名為hello.service：

```
hello.service

[Unit]
Description=My Service
After=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker kill hello
ExecStartPre=-/usr/bin/docker rm hello
ExecStartPre=/usr/bin/docker pull busybox
ExecStart=/usr/bin/docker run --name hello busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
ExecStop=/usr/bin/docker stop hello
```

然後，讀取並啟動這個單元：

```
$ fleetctl load hello.service
=> Unit hello.service loaded on 8145ebb7.../172.17.8.105
$ fleetctl start hello.service
=> Unit hello.service launched on 8145ebb7.../172.17.8.105
```

這樣，你的容器將在叢集裡被啟動。

下面我們 檢視下它的狀態：

```
$ fleetctl status hello.service
● hello.service - My Service
   Loaded: loaded (/run/fleet/units/hello.service; linked-runtime)
   Active: active (running) since Wed 2014-06-04 19:04:13 UTC; 44s ago
 Main PID: 27503 (bash)
   CGroup: /system.slice/hello.service
           ├─27503 /bin/bash -c /usr/bin/docker start -a hello || /usr/bin/docker run --name hello busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
           └─27509 /usr/bin/docker run --name hello busybox /bin/sh -c while true; do echo Hello World; sleep 1; done

Jun 04 19:04:57 core-01 bash[27503]: Hello World
..snip...
Jun 04 19:05:06 core-01 bash[27503]: Hello World
```

我們可以停止容器：

```
fleetctl destroy hello.service
```

至此，就是CoreOS提供的三大工具。
