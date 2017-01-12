## 使用 Rail

我們現在將使用 Compose 設定並執行一個 Rails/PostgreSQL 應用。在開始之前，先確保 Compose 已經 [安裝](install.md)。


在一切工作開始前，需要先設定好三個必要的檔案。  
首先，因為應用將要執行在一個滿足所有環境依賴的 Docker 容器裡面，那麼我們可以透過編輯 `Dockerfile` 檔案來指定 Docker 容器要安裝內容。內容如下： 

```
FROM ruby
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev
RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
RUN bundle install
ADD . /myapp
```
以上內容指定應用將使用安裝了 Ruby、Bundler 以及其依賴件的映像檔。更多關於如何編輯 Dockerfile 檔案的資訊可以 檢視 [映像檔建立](../image/create.md#利用 Dockerfile 來建立映像檔) 和 [Dockerfile 使用](../dockerfile/README.md)。
下一步，我們需要一個引導載入 Rails 的檔案 `Gemfile` 。 等一會兒它還會被 `rails new` 指令覆寫重寫。

```
source 'https://rubygems.org'
gem 'rails', '4.0.2'
```
最後，`docker-compose.yml` 檔案才是最神奇的地方。 `docker-compose.yml` 檔案將把所有的東西關聯起來。它描述了應用的構成（一個 web 服務和一個資料庫）、每個映像檔的來源（資料庫執行在使用預定義的 PostgreSQL 映像檔，web 應用側將從本地目錄建立）、映像檔之間的連線，以及服務開放的連接埠。

```
db:
  image: postgres
  ports:
    - "5432"
web:
  build: .
  command: bundle exec rackup -p 3000
  volumes:
    - .:/myapp
  ports:
    - "3000:3000"
  links:
    - db
```
所有檔案就緒後，我們就可以透過使用 `docker-compose run` 指令產生應用的骨架了。 
 
```
$ docker-compose run web rails new . --force --database=postgresql --skip-bundle
```
Compose 會先使用 `Dockerfile` 為 web 服務建立一個映像檔，接著使用這個映像檔在容器裡執行 `rails new ` 和它之後的指令。一旦這個指令執行完後，應該就可以看一個嶄新的應用已經產生了。

```
$ ls
Dockerfile   app          docker-compose.yml      tmp
Gemfile      bin          lib          vendor
Gemfile.lock condocker-compose       log
README.rdoc  condocker-compose.ru    public
Rakefile     db           test
```
在新的 `Gemfile` 檔案去掉載入 `therubyracer` 的行的註釋，這樣我們便可以使用 Javascript 執行環境：

```
gem 'therubyracer', platforms: :ruby
```
現在我們已經有一個新的 `Gemfile` 檔案，需要再重新建立映像檔。（這個會步驟會改變 Dockerfile 檔案本身，僅僅需要重建一次）。

```
$ docker-compose build
```
應用現在就可以啟動了，但設定還未完成。Rails  預設讀取的資料庫目標是 `localhost` ，我們需要手動指定容器的 `db` 。同樣的，還需要把使用者名稱修改成和 postgres 映像檔預定的一致。
開啟最新產生的 `database.yml` 檔案。用以下內容取代：

```
development: &default
  adapter: postgresql
  encoding: unicode
  database: postgres
  pool: 5
  username: postgres
  password:
  host: db

test:
  <<: *default
  database: myapp_test
```
現在就可以啟動應用了。

```
$ docker-compose up
```
如果一切正常，你應該可以看到 PostgreSQL 的輸出，幾秒後可以看到這樣的重複資訊：

```
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick 1.3.1
myapp_web_1 | [2014-01-17 17:16:29] INFO  ruby 2.0.0 (2013-11-22) [x86_64-linux-gnu]
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick::HTTPServer#start: pid=1 port=3000
```
最後， 我們需要做的是建立資料庫，開啟另一個終端機，執行：

```
$ docker-compose run web rake db:create
```
這個 web 應用已經開始在你的 docker 守護處理序裡面監聽著 3000 連接埠了（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的位址）。

![](../_images/docker-compose-rails-screenshot.png)
