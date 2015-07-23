## [MongoDB](https://registry.hub.docker.com/_/mongo/)

### �򥻫H��
[MongoDB](https://en.wikipedia.org/wiki/MongoDB) �O�}���� NoSQL �ƾڮw��{�C
�ӭܮw���ѤF MongoDB 2.2 ~ 2.7 �U�Ӫ������蹳�C

### �ϥΤ�k
�q�{�|�b `27017` �ݤf�Ұʼƾڮw�C
```
$ sudo docker run --name some-mongo -d mongo
```

�ϥΨ�L���γs����e���A�i�H��
```
$ sudo docker run --name some-app --link some-mongo:mongo -d application-that-uses-mongo
```
�Ϊ̳q�L `mongo`
```
$ sudo docker run -it --link some-mongo:mongo --rm mongo sh -c 'exec mongo "$MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT/test"'
```

### Dockerfile
* [2.2 ����](https://github.com/docker-library/mongo/blob/77c841472ccb6cc87fea1218269d097405edc6cb/2.2/Dockerfile)
* [2.4 ����](https://github.com/docker-library/mongo/blob/807078cb7b5f0289f6dabf9f6875d5318122bc30/2.4/Dockerfile)
* [2.6 ����](https://github.com/docker-library/mongo/blob/77c841472ccb6cc87fea1218269d097405edc6cb/2.6/Dockerfile)
* [2.7 ����](https://github.com/docker-library/mongo/blob/807078cb7b5f0289f6dabf9f6875d5318122bc30/2.7/Dockerfile)
