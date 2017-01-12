## 安裝
Docker Machine 可以在多種作業系統平台上安裝，包括 Linux、Mac OS，以及 Windows。


### Linux/Mac OS
在 Linux/Mac OS 上的安裝十分簡單，推薦從 [官方 Release 庫](https://github.com/docker/machine/releases) 直接下載編譯好的二進位檔案即可。

例如，在 Linux 64 位系統上直接下載對應的二進位包。
```bash
$ sudo curl -L https://github.com/docker/machine/releases/download/v0.3.1-rc1/docker-machine_linux-amd64 > /usr/local/bin/docker-machine
$ chmod +x /usr/local/bin/docker-machine
```

完成後， 檢視版本資訊，驗證執行正常。
```bash
$ docker-machine -v
docker-machine version 0.3.1-rc1 (993f2db)
```

### Windows
Windows 下面要複雜一些，首先需要安裝 [msysgit](https://msysgit.github.io/)。

msysgit 是 Windows 下的 git 用戶端套裝軟體，會提供類似 Linux 下的一些基本的工具，例如 ssh 等。

安裝之後，啟動 msysgit 的指令行指令列界面，仍然透過下載二進位包進行安裝，需要下載 docker 用戶端和 docker-machine。

```bash
$ curl -L https://get.docker.com/builds/Windows/x86_64/docker-latest.exe > /bin/docker

$ curl -L https://github.com/docker/machine/releases/download/v0.3.1-rc1/docker-machine_windows-amd64.exe > /bin/docker-machine
```
