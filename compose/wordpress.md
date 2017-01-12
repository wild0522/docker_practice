## 使用 Wordpress
Compose 讓 Wordpress 執行在一個獨立的環境中很簡易。

[安裝](install.md) Compose ，然後下載 Wordpress 到目前目錄：

```
wordpress.org/latest.tar.gz | tar -xvzf -
```
這將會建立一個叫 wordpress 目錄，你也可以重新命名成你想要的名字。在目錄裡面，建立一個 `Dockerfile` 檔案，定義應用的執行環境：

```
FROM orchardup/php5
ADD . /code
```
以上內容告訴 Docker 建立一個包含 PHP 和 Wordpress 的映像檔。更多關於如何編輯 Dockerfile 檔案的資訊可以 檢視 [映像檔建立](../image/create.md#利用 Dockerfile 來建立映像檔) 和 [Dockerfile 使用](../dockerfile/README.md)。


下一步，`docker-compose.yml` 檔案將開啟一個 web 服務和一個獨立的 MySQL 實例：

```
web:
  build: .
  command: php -S 0.0.0.0:8000 -t /code
  ports:
    - "8000:8000"
  links:
    - db
  volumes:
    - .:/code
db:
  image: orchardup/mysql
  environment:
    MYSQL_DATABASE: wordpress
```
要讓這個應用跑起來還需要兩個檔案。
第一個，`wp-condocker-compose.php` ，它是一個標準的 Wordpress 設定檔，有一點需要修改的是把資料庫的設定指向 `db` 容器。

```
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'root');
define('DB_PASSWORD', '');
define('DB_HOST', "db:3306");
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');

define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

$table_prefix  = 'wp_';
define('WPLANG', '');
define('WP_DEBUG', false);

if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__) . '/');

require_once(ABSPATH . 'wp-settings.php');
```
第二個，`router.php` ，它告訴 PHP 內建的伺服器怎麼執行 Wordpress:

```
<?php

$root = $_SERVER['DOCUMENT_ROOT'];
chdir($root);
$path = '/'.ltrim(parse_url($_SERVER['REQUEST_URI'])['path'],'/');
set_include_path(get_include_path().':'.__DIR__);
if(file_exists($root.$path))
{
    if(is_dir($root.$path) && substr($path,strlen($path) - 1, 1) !== '/')
        $path = rtrim($path,'/').'/index.php';
    if(strpos($path,'.php') === false) return false;
    else {
        chdir(dirname($root.$path));
        require_once $root.$path;
    }
}else include_once 'index.php';
```

這些設定檔就緒後，在你的 Wordpress 目錄裡面執行 `docker-compose up` 指令，Compose 就會拉取映像檔再建立我們所需要的映像檔，然後啟動 web 和資料庫容器。 接著存取 docker 守護處理序監聽的 8000 連接埠就能看你的 Wordpress 網站了。（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的位址）。

