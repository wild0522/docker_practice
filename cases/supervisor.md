## �ϥ� Supervisor �Ӻ޲z�i�{
Docker �e���b�Ұʪ��ɭԶ}�ҳ�Ӷi�{�A��p�A�@�� ssh �Ϊ� apache �� daemon �A�ȡC���ڭ̸g�`�ݭn�b�@�Ӿ����W�}�Ҧh�ӪA�ȡA�o�i�H���ܦh��k�A��²�檺�N�O��h�ӱҰʩR�O���@�ӱҰʸ}���̭��A�Ұʪ��ɭԪ����Ұʳo�Ӹ}���A�t�~�N�O�w�˶i�{�޲z�u��C

���p�`�N�ϥζi�{�޲z�u�� supervisor �Ӻ޲z�e�������h�Ӷi�{�C�ϥ� Supervisor �i�H��n������B�޲z�B���ҧڭ̧Ʊ�B�檺�i�{�C�b�o�̧ڭ̺t�ܤ@�U�p��P�ɨϥ� ssh �M apache �A�ȡC

### �t�m
�����Ыؤ@�� Dockerfile�A���e�M�U�����������p�U�C
```
FROM ubuntu:13.04
MAINTAINER examples@docker.com
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
```

### �w�� ssh�Bapache �M supervisor
```
RUN apt-get install -y openssh-server apache2 supervisor
RUN mkdir -p /var/run/sshd
RUN mkdir -p /var/log/supervisor
```

�o�̦w�� 3 �ӳn��A�ٳЫؤF 2 �� ssh �M supervisor �A�ȥ��`�B��һݭn���ؿ��C
```
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
```
�K�[ supervisord ���t�m���A�ýƻs�t�m��������ؿ��U���C

```
EXPOSE 22 80
CMD ["/usr/bin/supervisord"]
```
�o�̧ڭ̬M�g�F 22 �M 80 �ݤf�A�ϥ� supervisord ���i������w�ҰʪA�ȡC


### supervisor�t�m��󤺮e
```
[supervisord]
nodaemon=true
[program:sshd]
command=/usr/sbin/sshd -D

[program:apache2]
command=/bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"
```
�t�m���]�t�ؿ��M�i�{�A�Ĥ@�q supervsord �t�m�n�󥻨��A�ϥ� nodaemon �ѼƨӹB��C�ĤG�q�]�t�n��� 2 �ӪA�ȡC�C�@�q�]�t�@�ӪA�Ȫ��ؿ��M�Ұʳo�ӪA�Ȫ��R�O�C

### �ϥΤ�k
�Ы��蹳�C
```
$ sudo docker build -t test/supervisord .
```
�Ұ� supervisor �e���C
```
$ sudo docker run -p 22 -p 80 -t -i test/supervisord
2013-11-25 18:53:22,312 CRIT Supervisor running as root (no user in config file)
2013-11-25 18:53:22,312 WARN Included extra file "/etc/supervisor/conf.d/supervisord.conf" during parsing
2013-11-25 18:53:22,342 INFO supervisord started with pid 1
2013-11-25 18:53:23,346 INFO spawned: 'sshd' with pid 6
2013-11-25 18:53:23,349 INFO spawned: 'apache2' with pid 7
```
�ϥ� `docker run` �ӱҰʧڭ̳Ыت��e���C�ϥΦh�� `-p` �ӬM�g�h�Ӻݤf�A�o�˧ڭ̴N��P�ɳX�� ssh �M apache �A�ȤF�C

�i�H�ϥγo�Ӥ�k�Ыؤ@�ӥu�� ssh �A�Ȫ���¦�蹳�A����Ы��蹳�i�H�ϥγo���蹳����¦�ӳЫ�
