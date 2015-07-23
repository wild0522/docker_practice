## [Node.js](https://registry.hub.docker.com/_/node/)

### �򥻫H��
[Node.js](https://en.wikipedia.org/wiki/Node.js)�O��� JavaScript ���i�X�i�A�ȺݩM�����n��}�o���x�C
�ӭܮw���ѤF Node.js 0.8 ~ 0.11 �U�Ӫ������蹳�C

### �ϥΤ�k
�b���ؤ��Ыؤ@�� Dockerfile�C
```
FROM node:0.10-onbuild
# replace this with your application's default port
EXPOSE 8888
```
�M��Ы��蹳�A�ñҰʮe��
```
$ sudo docker build -t my-nodejs-app
$ sudo docker run -it --rm --name my-running-app my-nodejs-app
```

�]�i�H�����B��@��²��e���C
```
$ sudo docker run -it --rm --name my-running-script -v "$(pwd)":/usr/src/myapp -w /usr/src/myapp node:0.10 node your-daemon-or-script.js
```

### Dockerfile
* [0.8 ����](https://github.com/docker-library/node/blob/d017d679e92e84a810c580cdb29fcdbba23c2bb9/0.8/Dockerfile)
* [0.10 ����](https://github.com/docker-library/node/blob/913a225f2fda34d6a811fac1466e4f09f075fcf6/0.10/Dockerfile)
* [0.11 ����](https://github.com/docker-library/node/blob/d017d679e92e84a810c580cdb29fcdbba23c2bb9/0.11/Dockerfile)
