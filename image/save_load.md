## �s�X�M���J�蹳

### �s�X�蹳
�p�G�n�ɥX�蹳�쥻�a���A�i�H�ϥ� `docker save` �R�O�C
```
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              14.04               c4ff7513909d        5 weeks ago         225.4 MB
...
$sudo docker save -o ubuntu_14.04.tar ubuntu:14.04
```

### ���J�蹳
�i�H�ϥ� `docker load` �q�ɥX�����a��󤤦A�ɤJ�쥻�a�蹳�w�A�Ҧp
```
$ sudo docker load --input ubuntu_14.04.tar
```
��
```
$ sudo docker load < ubuntu_14.04.tar
```
�o�N�ɤJ�蹳�H�Ψ���������ƾګH���]�]�A���ҵ��^�C
