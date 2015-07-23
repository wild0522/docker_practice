## [Redis](https://registry.hub.docker.com/_/redis/)

### �򥻫H��
[Redis](https://en.wikipedia.org/wiki/Redis) �O�}�������s Key-Value �ƾڮw��{�C
�ӭܮw���ѤF Redis 2.6 ~ 2.8.9 �U�Ӫ������蹳�C

### �ϥΤ�k
�q�{�|�b `6379` �ݤf�Ұʼƾڮw�C
```
$ sudo docker run --name some-redis -d redis
```
�t�~�٥i�H�ҥ� [���[�s�x](http://redis.io/topics/persistence)�C
```
$ sudo docker run --name some-redis -d redis redis-server --appendonly yes
```
�q�{�ƾڦs�x��m�b `VOLUME/data`�C�i�H�ϥ� `--volumes-from some-volume-container` �� `-v /docker/host/dir:/data` �N�ƾڦs��쥻�a�C

�ϥΨ�L���γs����e���A�i�H��
```
$ sudo docker run --name some-app --link some-redis:redis -d application-that-uses-redis
```
�Ϊ̳q�L `redis-cli`
```
$ sudo docker run -it --link some-redis:redis --rm redis sh -c 'exec redis-cli -h "$REDIS_PORT_6379_TCP_ADDR" -p "$REDIS_PORT_6379_TCP_PORT"'
```

### Dockerfile
* [2.6 ����](https://github.com/docker-library/redis/blob/02d9cd887a4e0d50db4bb085eab7235115a6fe4a/2.6.17/Dockerfile)
* [�̷s 2.8 ����](https://github.com/docker-library/redis/blob/d0665bb1bbddd4cc035dbc1fc774695fa534d648/2.8.13/Dockerfile)
