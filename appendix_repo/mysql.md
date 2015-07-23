## [MySQL](https://registry.hub.docker.com/_/mysql/)

### �򥻫H��
[MySQL](https://en.wikipedia.org/wiki/MySQL) �O�}�������Y�ƾڮw��{�C
�ӭܮw���ѤF MySQL �U�Ӫ������蹳�A�]�A 5.6 �t�C�B5.7 �t�C���C

### �ϥΤ�k
�q�{�|�b `3306` �ݤf�Ұʼƾڮw�C
```
$ sudo docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql
```
����N�i�H�ϥΨ䥦���Ψӳs����Ӯe���C
```
$ sudo docker run --name some-app --link some-mysql:mysql -d application-that-uses-mysql
```
�Ϊ̳q�L `mysql`�C
```
$ sudo docker run -it --link some-mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
```

### Dockerfile
* [5.6 ����](https://github.com/docker-library/mysql/blob/7461a52b43f06839a4d8723ae8841f4cb616b3d0/5.6/Dockerfile)
* [5.7 ����](https://github.com/docker-library/mysql/blob/7461a52b43f06839a4d8723ae8841f4cb616b3d0/5.7/Dockerfile)
