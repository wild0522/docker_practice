##使用 Rail 入門 Fig

我們現在將使用 Fig 配置並運行一個 Rails/PostgreSQL 應用。在開始之前，先確保 Fig 已經 [安裝](install.md)。


在一切工作開始前，需要先設置好三個必要的文件。  
首先，因為應用將要運行在一個滿足所有環境依賴的 Docker 容器裡面，那麼我們可以通過編輯 `Dockerfile` 文件來指定 Docker 容器要安裝內容。內容如下： 

```
FROM ruby
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev
RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
RUN bundle install
ADD . /myapp
```
以上內容指定應用將使用安裝了 Ruby、Bundler 以及其依賴件的鏡像。更多關於如何編寫 Dockerfile 文件的信息可以查看 [鏡像創建](../image/create.md#利用 Dockerfile 來創建鏡像) 和 [Dockerfile 使用](../dockerfile/README.md)。
下一步，我們需要一個引導加載 Rails 的文件 `Gemfile` 。 等一會兒它還會被 `rails new` 命令覆蓋重寫。

```
source 'https://rubygems.org'
gem 'rails', '4.0.2'
```
最後，`fig.yml` 文件才是最神奇的地方。 `fig.yml` 文件將把所有的東西關聯起來。它描述了應用的構成（一個 web 服務和一個數據庫）、每個鏡像的來源（數據庫運行在使用預定義的 PostgreSQL 鏡像，web 應用側將從本地目錄創建）、鏡像之間的連接，以及服務開放的端口。

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
所有文件就緒後，我們就可以通過使用 `fig run` 命令生成應用的骨架了。 
 
```
$ fig run web rails new . --force --database=postgresql --skip-bundle
```
Fig 會先使用 `Dockerfile` 為 web 服務創建一個鏡像，接著使用這個鏡像在容器裡運行 `rails new ` 和它之後的命令。一旦這個命令運行完後，應該就可以看一個嶄新的應用已經生成了。

```
$ ls
Dockerfile   app          fig.yml      tmp
Gemfile      bin          lib          vendor
Gemfile.lock config       log
README.rdoc  config.ru    public
Rakefile     db           test
```
在新的 `Gemfile` 文件去掉加載 `therubyracer` 的行的註釋，這樣我們便可以使用 Javascript 運行環境：

```
gem 'therubyracer', platforms: :ruby
```
現在我們已經有一個新的 `Gemfile` 文件，需要再重新創建鏡像。（這個會步驟會改變 Dockerfile 文件本身，僅僅需要重建一次）。

```
$ fig build
```
應用現在就可以啟動了，但配置還未完成。Rails 默認讀取的數據庫目標是 `localhost` ，我們需要手動指定容器的 `db` 。同樣的，還需要把用戶名修改成和 postgres 鏡像預定的一致。
打開最新生成的 `database.yml` 文件。用以下內容替換：

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
$ fig up
```
如果一切正常，你應該可以看到 PostgreSQL 的輸出，幾秒後可以看到這樣的重複信息：

```
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick 1.3.1
myapp_web_1 | [2014-01-17 17:16:29] INFO  ruby 2.0.0 (2013-11-22) [x86_64-linux-gnu]
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick::HTTPServer#start: pid=1 port=3000
```
最後， 我們需要做的是創建數據庫，打開另一個終端，運行：

```
$ fig run web rake db:create
```
這個 web 應用已經開始在你的 docker 守護進程裡面監聽著 3000 端口了（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的地址）。

![](../_images/fig-rails-screenshot.png)