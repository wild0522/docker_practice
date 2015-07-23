##�ϥ� Wordpress �J�� Fig
Fig �� Wordpress �B��b�@�ӿW�ߪ����Ҥ���²���C
[�w��](install.md) Fig �A�M��U�� Wordpress ���e�ؿ��G

```
wordpress.org/latest.tar.gz | tar -xvzf -
```
�o�N�|�Ыؤ@�ӥs wordpress �ؿ��A�A�]�i�H���R�W���A�Q�n���W�r�C�b�ؿ��̭��A�Ыؤ@�� `Dockerfile` ���A�w�q���Ϊ��B�����ҡG

```
FROM orchardup/php5
ADD . /code
```
�H�W���e�i�D Docker �Ыؤ@�ӥ]�t PHP �M Wordpress ���蹳�C��h����p��s�g Dockerfile ��󪺫H���i�H�d�� [�蹳�Ы�](../image/create.md#�Q�� Dockerfile �ӳЫ��蹳) �M [Dockerfile �ϥ�](../dockerfile/README.md)�C


�U�@�B�A`fig.yml` ���N�}�Ҥ@�� web �A�ȩM�@�ӿW�ߪ� MySQL ��ҡG

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
�n���o�����ζ]�_���ٻݭn��Ӥ��C
�Ĥ@�ӡA`wp-config.php` �A���O�@�ӼзǪ� Wordpress �t�m���A���@�I�ݭn�ק諸�O��ƾڮw���t�m���V `db` �e���C

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
�ĤG�ӡA`router.php` �A���i�D PHP ���m���A�Ⱦ����B�� Wordpress:

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

�o�ǰt�m���N����A�b�A�� Wordpress �ؿ��̭����� `fig up` ���O�AFig �N�|�Ԩ��蹳�A�Ыاڭ̩һݭn���蹳�A�M��Ұ� web �M�ƾڮw�e���C ���۳X�� docker �u�@�i�{��ť�� 8000 �ݤf�N��ݧA�� Wordpress �����F�C�]�p�G�A���ϥ� boot2docker �A���� `boot2docker ip` �A�N�|�ݨ쥦���a�}�^�C

