## CentOS �t�C�w�� Docker

Docker ��� CentOS6 �ΥH�᪺�����C

### CentOS6
��� CentOS6�A�i�H�ϥ� [EPEL](https://fedoraproject.org/wiki/EPEL) �w�w�� Docker�A�R�O�p�U
```
$ sudo yum install http://mirrors.yun-idc.com/epel/6/i386/epel-release-6-8.noarch.rpm
$ sudo yum install docker-io
```

### CentOS7
CentOS7 �t�� `CentOS-Extras` �w���w�a Docker�A�i�H�����w�ˡG
```
$ sudo yum install docker
```

�w�ˤ���Ұ� Docker �A�ȡA�������H�t�αҰʦ۰ʥ[���C
```
$ sudo service docker start
$ sudo chkconfig docker on
```
