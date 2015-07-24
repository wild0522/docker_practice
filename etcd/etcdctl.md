## 使用 etcdctl

etcdctl 是一個命令行客戶端，它能提供一些簡潔的命令，供用戶直接跟 etcd 服務打交道，而無需基於 HTTP API 方式。這在某些情況下將很方便，例如用戶對服務進行測試或者手動修改數據庫內容。我們也推薦在剛接觸 etcd 時通過 etcdctl 命令來熟悉相關的操作，這些操作跟 HTTP API 實際上是對應的。

etcd 項目二進制發行包中已經包含了 etcdctl 工具，沒有的話，可以從 [github.com/coreos/etcd/releases](https://github.com/coreos/etcd/releases) 下載。

etcdctl 支持如下的命令，大體上分為數據庫操作和非數據庫操作兩類，後面將分別進行解釋。

```
$ etcdctl -h
NAME:
   etcdctl - A simple command line client for etcd.

USAGE:
   etcdctl [global options] command [command options] [arguments...]

VERSION:
   2.0.0-rc.1

COMMANDS:
   backup	backup an etcd directory
   mk		make a new key with a given value
   mkdir	make a new directory
   rm		remove a key
   rmdir	removes the key if it is an empty directory or a key-value pair
   get		retrieve the value of a key
   ls		retrieve a directory
   set		set the value of a key
   setdir	create a new or existing directory
   update	update an existing key with a given value
   updatedir	update an existing directory
   watch	watch a key for changes
   exec-watch	watch a key for changes and exec an executable
   member	member add, remove and list subcommands
   help, h	Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --debug			output cURL commands which can be used to reproduce the request
   --no-sync			don't synchronize cluster information before sending request
   --output, -o 'simple'	output response in the given format (`simple` or `json`)
   --peers, -C 			a comma-delimited list of machine addresses in the cluster (default: "127.0.0.1:4001")
   --cert-file 			identify HTTPS client using this SSL certificate file
   --key-file 			identify HTTPS client using this SSL key file
   --ca-file 			verify certificates of HTTPS-enabled servers using this CA bundle
   --help, -h			show help
   --version, -v		print the version
```

### 數據庫操作
數據庫操作圍繞對鍵值和目錄的 CRUD （符合 REST 風格的一套操作：Create）完整生命週期的管理。

etcd 在鍵的組織上採用了層次化的空間結構（類似於文件系統中目錄的概念），用戶指定的鍵可以為單獨的名字，如 `testkey`，此時實際上放在根目錄 `/` 下面，也可以為指定目錄結構，如 `cluster1/node2/testkey`，則將創建相應的目錄結構。

*註：CRUD 即 Create, Read, Update, Delete，是符合 REST 風格的一套 API 操作。*

#### set
指定某個鍵的值。例如
```
$ etcdctl set /testdir/testkey "Hello world"
Hello world
```
支持的選項包括：
```
--ttl '0'			該鍵值的超時時間（單位為秒），不配置（預設為 0）則永不超時
--swap-with-value value 若該鍵現在的值是 value，則進行設置操作
--swap-with-index '0'	若該鍵現在的索引值是指定索引，則進行設置操作
```

#### get
獲取指定鍵的值。例如
```
$ etcdctl set testkey hello
hello
$ etcdctl update testkey world
world
```

當鍵不存在時，則會報錯。例如
```
$ etcdctl get testkey2
Error:  100: Key not found (/testkey2) [1]
```

支持的選項為
```
--sort	對結果進行排序
--consistent 將請求發給主節點，保證獲取內容的一致性
```

#### update
當鍵存在時，更新值內容。例如
```
$ etcdctl set testkey hello
hello
$ etcdctl update testkey world
world
```

當鍵不存在時，則會報錯。例如
```
$ etcdctl update testkey2 world
Error:  100: Key not found (/testkey2) [1]
```

支持的選項為
```
--ttl '0'	超時時間（單位為秒），不配置（預設為 0）則永不超時
```

#### rm
刪除某個鍵值。例如
```
$ etcdctl rm testkey

```

當鍵不存在時，則會報錯。例如
```
$ etcdctl rm testkey2
Error:  100: Key not found (/testkey2) [8]
```

支持的選項為
```
--dir		如果鍵是個空目錄或者鍵值對則刪除
--recursive		刪除目錄和所有子鍵
--with-value 	檢查現有的值是否匹配
--with-index '0'	檢查現有的 index 是否匹配

```

#### mk
如果給定的鍵不存在，則創建一個新的鍵值。例如
```
$ etcdctl mk /testdir/testkey "Hello world"
Hello world
```
當鍵存在的時候，執行該命令會報錯，例如
```
$ etcdctl set testkey "Hello world"
Hello world
$ ./etcdctl mk testkey "Hello world"
Error:  105: Key already exists (/testkey) [2]
```

支持的選項為
```
--ttl '0'	超時時間（單位為秒），不配置（預設為 0）則永不超時
```


#### mkdir
如果給定的鍵目錄不存在，則創建一個新的鍵目錄。例如
```
$ etcdctl mkdir testdir
```
當鍵目錄存在的時候，執行該命令會報錯，例如
```
$ etcdctl mkdir testdir
$ etcdctl mkdir testdir
Error:  105: Key already exists (/testdir) [7]
```
支持的選項為
```
--ttl '0'	超時時間（單位為秒），不配置（預設為 0）則永不超時
```

#### setdir

創建一個鍵目錄，無論存在與否。

支持的選項為
```
--ttl '0'	超時時間（單位為秒），不配置（預設為 0）則永不超時
```

#### updatedir
更新一個已經存在的目錄。
支持的選項為
```
--ttl '0'	超時時間（單位為秒），不配置（預設為 0）則永不超時
```

#### rmdir
刪除一個空目錄，或者鍵值對。

若目錄不空，會報錯
```
$ etcdctl set /dir/testkey hi
hi
$ etcdctl rmdir /dir
Error:  108: Directory not empty (/dir) [13]
```

#### ls
列出目錄（預設為根目錄）下的鍵或者子目錄，預設不顯示子目錄中內容。

例如
```
$ ./etcdctl set testkey 'hi'
hi
$ ./etcdctl set dir/test 'hello'
hello
$ ./etcdctl ls
/testkey
/dir
$ ./etcdctl ls dir
/dir/test
```

支持的選項包括
```
--sort	將輸出結果排序
--recursive	如果目錄下有子目錄，則遞歸輸出其中的內容
-p		對於輸出為目錄，在最後添加 `/` 進行區分
```

### 非數據庫操作

#### backup
備份 etcd 的數據。

支持的選項包括
```
--data-dir 		etcd 的數據目錄
--backup-dir 	備份到指定路徑
```
#### watch
監測一個鍵值的變化，一旦鍵值發生更新，就會輸出最新的值並退出。

例如，用戶更新 testkey 鍵值為 Hello world。
```
$ etcdctl watch testkey
Hello world
```

支持的選項包括
```
--forever		一直監測，直到用戶按 `CTRL+C` 退出
--after-index '0'	在指定 index 之前一直監測
--recursive		返回所有的鍵值和子鍵值
```
#### exec-watch
監測一個鍵值的變化，一旦鍵值發生更新，就執行給定命令。

例如，用戶更新 testkey 鍵值。
```
$etcdctl exec-watch testkey -- sh -c 'ls'
default.etcd
Documentation
etcd
etcdctl
etcd-migrate
README-etcdctl.md
README.md
```

支持的選項包括
```
--after-index '0'	在指定 index 之前一直監測
--recursive		返回所有的鍵值和子鍵值
```

#### member
通過 list、add、remove 命令列出、添加、刪除 etcd 實例到 etcd 集群中。

例如本地啟動一個 etcd 服務實例後，可以用如下命令進行查看。
```
$ etcdctl member list
ce2a822cea30bfca: name=default peerURLs=http://localhost:2380,http://localhost:7001 clientURLs=http://localhost:2379,http://localhost:4001

```
### 命令選項
* `--debug`			輸出 cURL 命令，顯示執行命令的時候發起的請求
* `--no-sync`			發出請求之前不同步集群信息
* `--output, -o 'simple'`	輸出內容的格式 (`simple` 為原始信息，`json` 為進行json格式解碼，易讀性好一些)
* `--peers, -C`			指定集群中的同伴信息，用逗號隔開 (預設為: "127.0.0.1:4001")
* `--cert-file` 			HTTPS 下客戶端使用的 SSL 證書文件
* `--key-file`			HTTPS 下客戶端使用的 SSL 密鑰文件
* `--ca-file` 			服務端使用 HTTPS 時，使用 CA 文件進行驗證
* `--help, -h`			顯示幫助命令信息
* `--version, -v`		打印版本信息
