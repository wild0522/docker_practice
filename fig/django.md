##使用 Django 入門 Fig

我們現在將使用 Fig 配置並運行一個 Django/PostgreSQL 應用。在此之前，先確保 Fig 已經 [安裝](install.md)。

在一切工作開始前，需要先設置好三個必要的文件。  
第一步，因為應用將要運行在一個滿足所有環境依賴的 Docker 容器裡面，那麼我們可以通過編輯 `Dockerfile` 文件來指定 Docker 容器要安裝內容。內容如下： 

```
FROM python:2.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```
以上內容指定應用將使用安裝了 Python 以及必要依賴包的鏡像。更多關於如何編寫 Dockerfile 文件的信息可以查看 [鏡像創建](../image/create.md#利用 Dockerfile 來創建鏡像) 和 [Dockerfile 使用](../dockerfile/README.md)。

第二步，在 `requirements.txt` 文件裡面寫明需要安裝的具體依賴包名 。

```
Django
psycopg2
```

就是這麼簡單。  
第三步，`fig.yml` 文件將把所有的東西關聯起來。它描述了應用的構成（一個 web 服務和一個數據庫）、使用的 Docker 鏡像、鏡像之間的連接、掛載到容器的卷，以及服務開放的連接阜。 

```
db:
  image: postgres
web:
  build: .
  command: python manage.py runserver 0.0.0.0:8000
  volumes:
    - .:/code
  ports:
    - "8000:8000"
  links:
    - db
```
查看 [`fig.yml` 章節](yml_ref.md) 瞭解更多詳細的工作機制。

現在我們就可以使用 `fig run` 命令啟動一個 Django 應用了。

```
$ fig run web django-admin.py startproject figexample .
```
Fig 會先使用 `Dockerfile` 為 web 服務創建一個鏡像，接著使用這個鏡像在容器裡運行 `django-admin.py startproject figexample . ` 指令。

這將在當前目錄生成一個 Django 應用。

```
$ ls
Dockerfile       fig.yml          figexample       manage.py       requirements.txt
```
首先，我們要為應用設置好數據庫的連接信息。用以下內容替換 `figexample/settings.py` 文件中 `DATABASES = ...` 定義的節點內容。

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```
這些信息是在 [postgres](https://registry.hub.docker.com/_/postgres/) Docker 鏡像固定設置好的。  
然後，運行 `fig up` ：
	
```
Recreating myapp_db_1...
Recreating myapp_web_1...
Attaching to myapp_db_1, myapp_web_1
myapp_db_1 |
myapp_db_1 | PostgreSQL stand-alone backend 9.1.11
myapp_db_1 | 2014-01-27 12:17:03 UTC LOG:  database system is ready to accept connections
myapp_db_1 | 2014-01-27 12:17:03 UTC LOG:  autovacuum launcher started
myapp_web_1 | Validating models...
myapp_web_1 |
myapp_web_1 | 0 errors found
myapp_web_1 | January 27, 2014 - 12:12:40
myapp_web_1 | Django version 1.6.1, using settings 'figexample.settings'
myapp_web_1 | Starting development server at http://0.0.0.0:8000/
myapp_web_1 | Quit the server with CONTROL-C.
```
這個 web 應用已經開始在你的 docker 守護進程裡監聽著 5000 連接阜了（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的地址）。

你還可以在 Docker 上運行其它的管理命令，例如對於同步數據庫結構這種事，在運行完 `fig up` 後，在另外一個終端運行以下命令即可：

```
$ fig run web python manage.py syncdb
```