## ����蹳

�i�H�ϥ� `docker pull` �R�O�ӱq�ܮw����һݭn���蹳�C

�U�����Ҥl�N�q Docker Hub �ܮw�U���@�� Ubuntu 12.04 �ާ@�t�Ϊ��蹳�C
```
$ sudo docker pull ubuntu:12.04
Pulling repository ubuntu
ab8e2728644c: Pulling dependent layers
511136ea3c5a: Download complete
5f0ffaa9455e: Download complete
a300658979be: Download complete
904483ae0c30: Download complete
ffdaafd1ca50: Download complete
d047ae21eeaf: Download complete
```
�U���L�{���A�|��X����蹳���C�@�h�H���C

�өR�O��ڤW�۷�� `$ sudo docker pull registry.hub.docker.com/ubuntu:12.04` �R�O�A�Y�q���U�A�Ⱦ� `registry.hub.docker.com` ���� `ubuntu` �ܮw�ӤU���аO�� `12.04` ���蹳�C

���ɭԩx��ܮw���U�A�Ⱦ��U�����C�A�i�H�q��L�ܮw�U���C
�q�䥦�ܮw�U���ɻݭn���w���㪺�ܮw���U�A�Ⱦ��a�}�C�Ҧp
```
$ sudo docker pull dl.dockerpool.com:5000/ubuntu:12.04
Pulling dl.dockerpool.com:5000/ubuntu
ab8e2728644c: Pulling dependent layers
511136ea3c5a: Download complete
5f0ffaa9455e: Download complete
a300658979be: Download complete
904483ae0c30: Download complete
ffdaafd1ca50: Download complete
d047ae21eeaf: Download complete
```

������A�Y�i�H�ɨϥθ��蹳�F�A�Ҧp�Ыؤ@�Ӯe���A���䤤�B�� bash ���ΡC
```
$ sudo docker run -t -i ubuntu:12.04 /bin/bash
root@fe7fc4bd8fc9:/#
```
