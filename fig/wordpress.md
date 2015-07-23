##使用 Wordpress 入門 Fig
Fig 讓 Wordpress 運行在一個獨立的環境中很簡易。
[安裝](install.md) Fig ，然後下載 Wordpress 到當前目錄：

```
wordpress.org/latest.tar.gz | tar -xvzf -
```
這將會創建一個叫 wordpress 目錄，你也可以重命名成你想要的名字。在目錄裡面，創建一個 `Dockerfile` 文件，定義應用的運行環境：

```
FROM orchardup/php5
ADD . /code
```
以上內容告訴 Docker 創建一個包含 PHP 和 Wordpress 的鏡像。更多關於如何編寫 Dockerfile 文件的信息可以查看 [鏡像創建](../image/create.md#利用 Dockerfile 來創建鏡像) 和 [Dockerfile 使用](../dockerfile/README.md)。


下一步，`fig.yml` 文件將開啟一個 web 服務和一個獨立的 MySQL 實例：

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
要讓這個應用跑起來還需要兩個文件。
第一個，`wp-config.php` ，它是一個標準的 Wordpress 配置文件，有一點需要修改的是把數據庫的配置指向 `db` 容器。

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
第二個，`router.php` ，它告訴 PHP 內置的服務器怎麼運行 Wordpress:

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

這些配置文件就緒後，在你的 Wordpress 目錄裡面執行 `fig up` 指令，Fig 就會拉取鏡像再創建我們所需要的鏡像，然後啟動 web 和數據庫容器。 接著訪問 docker 守護進程監聽的 8000 端口就能看你的 Wordpress 網站了。（如果你有使用 boot2docker ，執行 `boot2docker ip` ，就會看到它的地址）。

