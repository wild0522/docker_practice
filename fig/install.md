##安裝 Fig

首先，安裝 1.3 或是更新的 Docker 版本。

如果你的工作環境是 OS X ，可以透過 檢視 [Mac 安裝指南(英文)](https://docs.docker.com/installation/mac/) ，完成安裝 Docker 和 boot2docker 。一旦 boot2docker 執行後，執行以下指令設定一個環境變數，接著 Fig 就可以和它互動了。

```
$(boot2docker shellinit)
```
**如果想避免重啟後重新設定，可以把上面的指令加到你的 ` ~/.bashrc` 檔案裡。*

關於 `Ubuntu` 還有 `其它的平台` 的安裝，可以參照 [Ubuntu 安裝指南(中文)](../install/ubuntu.md) 以及 [官方安裝手冊(英文)](https://docs.docker.com/installation/)。


下一步，安裝 Fig ：

```
curl -L https://github.com/docker/fig/releases/download/1.0.1/fig-`uname -s`-`uname -m` > /usr/local/bin/fig; chmod +x /usr/local/bin/fig
```
**如果你的 Docker 是管理員身份安裝，以上指令可能也需要相同的身份。*

目前 Fig 的發行版本只支援 OSX 和 64 位的 Linux 系統。但因為它是用 Python 語言寫的，所以對於其它平台上的使用者，可以透過 Python 安裝包來完成安裝（支援的系統同樣適用）。

```
$ sudo pip install -U fig
```
到這裡就已經完成了。 執行 `fig --version` ，確認能夠正常執行。

