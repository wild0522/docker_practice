#快速搭建CoreOS叢集

在這裡我們要搭建一個叢集環境，畢竟單機環境沒有什麼挑戰不是？

然後為了在你的電腦執行一個叢集環境，我們使用Vagrant。

*Vagrant的使用這裡不再闡述，請自行學習*

如果你第一次接觸CoreOS這樣的分散式平台，執行一個叢集看起來好像一個很複雜的工作，這裡我們給你展示在本地快速搭建一個CoreOS叢集環境是多麼的容易。

##準備工作

首先要確認在你本地的機器上已經安裝了最新版本的Virtualbox, Vagrant 和 git。

這是我們可以在本地類比叢集環境的前提條件，如果你已經擁有，請繼續，否則自行搜尋學習。

##設定工作

從CoreOS官方代碼程式庫取得基本設定，並進行修改

首先，取得 範本設定檔

```
git clone https://github.com/coreos/coreos-vagrant
cd coreos-vagrant
cp user-data.sample user-data
```

取得新的token

```
curl https://discovery.etcd.io/new
```

把取得的token放到user-data檔案中，範例如下：

```
#cloud-config

coreos:
  etcd:
    discovery: https://discovery.etcd.io/<token>
```

##啟動叢集

 預設情況下，CoreOS Vagrantfile 將會啟動單機。

我們需要複製並修改config.rb.sample檔案.

複製檔案

```
cp config.rb.sample config.rb
```

修改叢集設定參數num_instances為3。

啟動叢集

```
vagrant up
=>
Bringing machine 'core-01' up with 'virtualbox' provider...
Bringing machine 'core-02' up with 'virtualbox' provider...
Bringing machine 'core-03' up with 'virtualbox' provider...
==> core-01: Box 'coreos-alpha' could not be found. Attempting to find and install...
    core-01: Box Provider: virtualbox
    core-01: Box Version: >= 0
==> core-01: Adding box 'coreos-alpha' (v0) for provider: virtualbox
    core-01: Downloading: http://storage.core-os.net/coreos/amd64-usr/alpha/coreos_production_vagrant.box
    core-01: Progress: 46% (Rate: 6105k/s, Estimated time remaining: 0:00:16)
```

加入ssh的公匙

```
ssh-add ~/.vagrant.d/insecure_private_key
```

連線叢集中的第一台機器

```
vagrant ssh core-01 -- -A
```

##測試叢集

使用fleet來 檢視機器執行狀況

```
fleetctl list-machines
=>
MACHINE   IP            METADATA
517d1c7d... 172.17.8.101  -
cb35b356... 172.17.8.103  -
17040743... 172.17.8.102  -
```

如果你也看到了如上類似的資訊，恭喜，本地基於三台機器的叢集已經成功啟動，是不是很簡單。

那麼之後你就可以基於CoreOS的三大工具做工作分發，分散式儲存等很多功能了。