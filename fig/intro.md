##快速搭建基於 Docker 的隔離開發環境

使用 `Dockerfile` 文件指定你的應用環境，讓它能在任意地方複製使用：

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
```

在 `fig.yml` 文件中指定應用使用的不同服務，讓它們能夠在一個獨立的環境中一起運行：

```
web:
  build: .
  command: python app.py
  links:
   - db
  ports:
   - "8000:8000"
db:
  image: postgres
```
**注意不需要再額外安裝 Postgres 了！*

接著執行命令 `fig up` ，然後 Fig 就會啟動並運行你的應用了。

![Docker](../_images/fig-example-large.gif)

Fig 可用的命令有:

* 啟動、停止，和重建服務
* 查看服務的運行狀態
* 查看運行中的服務的輸入日誌
* 對服務發送命令

##快速上手
我們試著讓一個基本的 Python web 應用運行在 Fig 上。這個實驗假設你已經知道一些 Python 知識，如果你不熟悉，但清楚概念上的東西也是沒有問題的。

首先，[安裝 Docker 和 Fig](install.md)

為你的項目創建一個目錄

```
$ mkdir figtest
$ cd figtest
```
進入目錄，創建 `app.py`，這是一個能夠讓 Redis 上的一個值自增的簡單 web 應用，基於 Flask 框架。

```
from flask import Flask
from redis import Redis
import os
app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    return 'Hello World! I have been seen %s times.' % redis.get('hits')

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```
在 `requirements.txt` 文件中指定應用的 Python 依賴包。

```
flask
redis
```
下一步我們要創建一個包含應用所有依賴的 Docker 鏡像，這裡將闡述怎麼通過 `Dockerfile` 文件來創建。

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
```
以上的內容首先告訴 Docker 在容器裡面安裝 Python ，代碼的路徑還有Python 依賴包。關於 Dockerfile 的更多信息可以查看 [鏡像創建](../image/create.md#利用 Dockerfile 來創建鏡像) 和 [Dockerfile 使用](../dockerfile/README.md)

接著我們通過 `fig.yml` 文件指定一系列的服務：

```
web:
  build: .
  command: python app.py
  ports:
   - "5000:5000"
  volumes:
   - .:/code
  links:
   - redis
redis:
  image: redis
  ```
這裡指定了兩個服務：

* web 服務，通過當前目錄的 `Dockerfile` 創建。並且說明了在容器裡面執行`python app.py ` 命令 ，轉發在容器裡開放的 5000 端口到本地主機的 5000 端口，連接 Redis 服務，並且掛載當前目錄到容器裡面，這樣我們就可以不用重建鏡像也能直接使用代碼。
* redis 服務，我們使用公用鏡像 [redis](https://registry.hub.docker.com/_/redis/)。
*
現在如果執行 `fig up` 命令 ，它就會拉取 redis 鏡像，啟動所有的服務。

```
$ fig up
Pulling image redis...
Building web...
Starting figtest_redis_1...
Starting figtest_web_1...
redis_1 | [8] 02 Jan 18:43:35.576 # Server started, Redis version 2.8.3
web_1   |  * Running on http://0.0.0.0:5000/
```
這個 web 應用已經開始在你的 docker 守護進程裡面監聽著 5000 端口了（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的地址）。

如果你想要在後台運行你的服務，可以在執行 `fig up` 命令的時候添加 `-d` 參數，然後使用 `fig ps` 查看有什麼進程在運行。

```
$ fig up -d
Starting figtest_redis_1...
Starting figtest_web_1...
$ fig ps
        Name                 Command            State       Ports
-------------------------------------------------------------------
figtest_redis_1   /usr/local/bin/run         Up
figtest_web_1     /bin/sh -c python app.py   Up      5000->5000/tcp
```

`fig run` 指令可以幫你向服務發送命令。例如：查看 web 服務可以獲取到的環境變量:

```
$ fig run web env
```
執行幫助命令 `fig --help` 查看其它可用的參數。

假設你使用了 `fig up -d` 啟動 Fig，可以通過以下命令停止你的服務：

```
$ fig stop
```
以上內容或多或少的講述了如何使用Fig 。通過查看下面的引用章節可以瞭解到關於命令、配置和環境變量的更多細節。如果你有任何想法或建議，[可以在 GitHub 上提出](https://github.com/docker/fig)。

