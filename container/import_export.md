##�ɥX�M�ɤJ�e��

###�ɥX�e��
�p�G�n�ɥX���a�Y�Ӯe���A�i�H�ϥ� `docker export` �R�O�C
```
$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:14.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ sudo docker export 7691a814370e > ubuntu.tar
```
�o�˱N�ɥX�e���ַӨ쥻�a���C

###�ɤJ�e���ַ�
�i�H�ϥ� `docker import` �q�e���ַӤ�󤤦A�ɤJ���蹳�A�Ҧp
```
$ cat ubuntu.tar | sudo docker import - test/ubuntu:v1.0
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
```
���~�A�]�i�H�q�L���w URL �Ϊ̬Y�ӥؿ��ӾɤJ�A�Ҧp
```
$sudo docker import http://example.com/exampleimage.tgz example/imagerepo
```

*���G�Τ�J�i�H�ϥ� `docker load` �ӾɤJ�蹳�s�x���쥻�a�蹳�w�A�]�i�H�ϥ� `docker import` �ӾɤJ�@�Ӯe���ַӨ쥻�a�蹳�w�C�o��̪��ϧO�b��e���ַӤ��N���Ҧ������v�O���M���ƾګH���]�Y�ȫO�s�e����ɪ��ַӪ��A�^�A���蹳�s�x���N�O�s����O���A��n�]�n�j�C���~�A�q�e���ַӤ��ɤJ�ɥi�H���s���w���ҵ����ƾګH���C


