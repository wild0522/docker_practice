## 使用 Django

我們現在將使用 Compose 設定並執行一個 Django/PostgreSQL 應用。在此之前，先確保 Compose 已經 [安裝](install.md)。

在一切工作開始前，需要先設定好三個必要的檔案。  
第一步，因為應用將要執行在一個滿足所有環境依賴的 Docker 容器裡面，那麼我們可以透過編輯 `Dockerfile` 檔案來指定 Docker 容器要安裝內容。內容如下： 

```
FROM python:2.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```
以上內容指定應用將使用安裝了 Python 以及必要依賴包的映像檔。更多關於如何編輯 Dockerfile 檔案的資訊可以 檢視 [映像檔建立](../image/create.md#利用 Dockerfile 來建立映像檔) 和 [Dockerfile 使用](../dockerfile/README.md)。

第二步，在 `requirements.txt` 檔案裡面寫明需要安裝的具體依賴包名 。

```
Django
psycopg2
```

就是這麼簡單。  
第三步，`docker-compose.yml` 檔案將把所有的東西關聯起來。它描述了應用的構成（一個 web 服務和一個資料庫）、使用的 Docker 映像檔、映像檔之間的連線、載入到容器的卷，以及服務開放的連接埠。 

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
 檢視 [`docker-compose.yml` 章節](yml_ref.md) 瞭解更多詳細的工作機制。

現在我們就可以使用 `docker-compose run` 指令啟動一個 Django 應用了。

```
$ docker-compose run web django-admin.py startproject docker-composeexample .
```
Compose 會先使用 `Dockerfile` 為 web 服務建立一個映像檔，接著使用這個映像檔在容器裡執行 `django-admin.py startproject docker-composeexample . ` 指令。

這將在目前目錄產生一個 Django 應用。

```
$ ls
Dockerfile       docker-compose.yml          docker-composeexample       manage.py       requirements.txt
```
首先，我們要為應用設定好資料庫的連線資訊。用以下內容取代 `docker-composeexample/settings.py` 檔案中 `DATABASES = ...` 定義的節點內容。

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
這些資訊是在 [postgres](https://registry.hub.docker.com/_/postgres/) Docker 映像檔固定設定好的。  
然後，執行 `docker-compose up` ：
	
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
myapp_web_1 | Django version 1.6.1, using settings 'docker-composeexample.settings'
myapp_web_1 | Starting development server at http://0.0.0.0:8000/
myapp_web_1 | Quit the server with CONTROL-C.
```
這個 web 應用已經開始在你的 docker 守護處理序裡監聽著 5000 連接埠了（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的位址）。

你還可以在 Docker 上執行其它的管理指令，例如對於同步資料庫結構這種事，在執行完 `docker-compose up` 後，在另外一個終端機執行以下指令即可：

```
$ docker-compose run web python manage.py syncdb
```
