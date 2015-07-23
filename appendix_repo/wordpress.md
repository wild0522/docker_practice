## [WordPress](https://registry.hub.docker.com/_/wordpress/)

### �򥻫H��
[WordPress](https://en.wikipedia.org/wiki/WordPress) �O�}���� Blog �M���e�޲z�t�ήج[�A����� PhP �M MySQL�C
�ӭܮw���ѤF WordPress 4.0 �������蹳�C

### �ϥΤ�k
�Ұʮe���ݭn MySQL ������A�q�{�ݤf�� `80`�C
```
$ sudo docker run --name some-wordpress --link some-mysql:mysql -d wordpress
```
�Ұ� WordPress �e���ɥi�H���w���@�����ҰѼƥ]�A
* `-e WORDPRESS_DB_USER=...` �ʬ٬� �uroot�v
* `-e WORDPRESS_DB_PASSWORD=...` �ʬ٬��s�� mysql �e���������ܶq `MYSQL_ROOT_PASSWORD` ����
* `-e WORDPRESS_DB_NAME=...` �ʬ٬� �uwordpress�v
* `-e WORDPRESS_AUTH_KEY=...`, `-e WORDPRESS_SECURE_AUTH_KEY=...`, `-e WORDPRESS_LOGGED_IN_KEY=...`, `-e WORDPRESS_NONCE_KEY=...`, `-e WORDPRESS_AUTH_SALT=...`, `-e WORDPRESS_SECURE_AUTH_SALT=...`, `-e WORDPRESS_LOGGED_IN_SALT=...`, `-e WORDPRESS_NONCE_SALT=...` �ʬ٬��H�� sha1 ��

### Dockerfile
* [4.0 ����](https://github.com/docker-library/wordpress/blob/aee00669e7c43f435f021cb02871bffd63d5677a/Dockerfile)
