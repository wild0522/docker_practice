##�Ы��蹳

�Ы��蹳���ܦh��k�A�Τ�i�H�q Docker Hub ����w���蹳�ç�s�A�]�i�H�Q�Υ��a���t�γЫؤ@�ӡC

### �ק�w���蹳
���ϥΤU�����蹳�Ұʮe���C
```
$ sudo docker run -t -i training/sinatra /bin/bash
root@0b2616b0e5a8:/#
```
�`�N�G�O��e���� ID�A�y���ٷ|�Ψ�C

�b�e�����K�[ json �M gem ������ΡC
```
root@0b2616b0e5a8:/# gem install json
```
������A�ڭ̨ϥ� exit �Ӱh�X�A�{�b�ڭ̪��e���w�g�Q�ڭ̧��ܤF�A�ϥ� `docker commit` �R�O�Ӵ����s�᪺�ƥ��C
```
$ sudo docker commit -m "Added json gem" -a "Docker Newbee" 0b2616b0e5a8 ouruser/sinatra:v2
4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
```
�䤤�A`-m` �ӫ��w���檺�����H���A��ڭ̨ϥΪ���������u��@�ˡF`-a` �i�H���w��s���Τ�H���F����O�ΨӳЫ��蹳���e���� ID�F�̫���w�ؼ��蹳���ܮw�W�M tag �H���C�Ыئ��\��|��^�o���蹳�� ID �H���C


�ϥ� `docker images` �Ӭd�ݷs�Ыت��蹳�C
```
$ sudo docker images
REPOSITORY          TAG     IMAGE ID       CREATED       VIRTUAL SIZE
training/sinatra    latest  5bc342fa0b91   10 hours ago  446.7 MB
ouruser/sinatra     v2      3c59e02ddd1a   10 hours ago  446.7 MB
ouruser/sinatra     latest  5db5f8471261   10 hours ago  446.7 MB
```
����A�i�H�ϥηs���蹳�ӱҰʮe��
```
$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
root@78e82f680994:/#
```

###�Q�� Dockerfile �ӳЫ��蹳
�ϥ� `docker commit` ���X�i�@���蹳���²��A���O����K�b�@�ӹζ������ɡC�ڭ̥i�H�ϥ� `docker build` �ӳЫؤ@�ӷs���蹳�C�����A�����ݭn�Ыؤ@�� Dockerfile�A�]�t�@�Ǧp��Ы��蹳�����O�C

�s�ؤ@�ӥؿ��M�@�� Dockerfile
```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```
Dockerfile ���C�@�����O���Ы��蹳���@�h�A�Ҧp�G
```
# This is a comment
FROM ubuntu:14.04
MAINTAINER Docker Newbee <newbee@docker.com>
RUN apt-get -qq update
RUN apt-get -qqy install ruby ruby-dev
RUN gem install sinatra
```
Dockerfile �򥻪��y�k�O
* �ϥ�`#`�ӵ���
* `FROM` ���O�i�D Docker �ϥέ����蹳�@����¦
* ���۬O���@�̪��H��
* `RUN`�}�Y�����O�|�b�Ыؤ��B��A��p�w�ˤ@�ӳn��]�A�b�o�̨ϥ� apt-get �Ӧw�ˤF�@�ǳn��

�s�g���� Dockerfile ��i�H�ϥ� `docker build` �ӥͦ��蹳�C

```
$ sudo docker build -t="ouruser/sinatra:v2" .
Uploading context  2.56 kB
Uploading context
Step 0 : FROM ubuntu:14.04
 ---> 99ec81b80c55
Step 1 : MAINTAINER Newbee <newbee@docker.com>
 ---> Running in 7c5664a8a0c1
 ---> 2fa8ca4e2a13
Removing intermediate container 7c5664a8a0c1
Step 2 : RUN apt-get -qq update
 ---> Running in b07cc3fb4256
 ---> 50d21070ec0c
Removing intermediate container b07cc3fb4256
Step 3 : RUN apt-get -qqy install ruby ruby-dev
 ---> Running in a5b038dd127e
Selecting previously unselected package libasan0:amd64.
(Reading database ... 11518 files and directories currently installed.)
Preparing to unpack .../libasan0_4.8.2-19ubuntu1_amd64.deb ...
Setting up ruby (1:1.9.3.4) ...
Setting up ruby1.9.1 (1.9.3.484-2ubuntu1) ...
Processing triggers for libc-bin (2.19-0ubuntu6) ...
 ---> 2acb20f17878
Removing intermediate container a5b038dd127e
Step 4 : RUN gem install sinatra
 ---> Running in 5e9d0065c1f7
. . .
Successfully installed rack-protection-1.5.3
Successfully installed sinatra-1.4.5
4 gems installed
 ---> 324104cde6ad
Removing intermediate container 5e9d0065c1f7
Successfully built 324104cde6ad
```
�䤤 `-t` �аO�ӲK�[ tag�A���w�s���蹳���Τ�H���C
�u.�v �O Dockerfile �Ҧb�����|�]��e�ؿ��^�A�]�i�H�������@�Ө��骺 Dockerfile �����|�C

�i�H�ݨ� build �i�{�b����ާ@�C���n�����Ĥ@��Ʊ��N�O�W�ǳo�� Dockerfile ���e�A�]���Ҧ����ާ@���n�̾� Dockerfile �Ӷi��C
�M��ADockfile �������O�Q�@���@��������C�C�@�B���ЫؤF�@�ӷs���e���A�b�e����������O�ô���ק�]�N�򤧫e���йL�� `docker commit` �@�ˡ^�C��Ҧ������O�����槹������A��^�F�̲ת��蹳 id�C�Ҧ��������B�J�Ҳ��ͪ��e�����Q�R���M�M�z�F�C

*�`�N�@���蹳����W�L 127 �h

���~�A�٥i�H�Q�� `ADD` �R�O�ƻs���a�����蹳�F�� `EXPOSE` �R�O�ӦV�~���}��ݤf�F�� `CMD` �R�O�Ӵy�z�e���Ұʫ�B�檺�{�ǵ��C�Ҧp
```
# put my local web site in myApp folder to /var/www
ADD myApp /var/www
# expose httpd port
EXPOSE 80
# the command to run
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```

�{�b�i�H�Q�ηs�Ыت��蹳�ӱҰʤ@�Ӯe���C
```
$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
root@8196968dac35:/#
```
�٥i�H�� `docker tag` �R�O�ӭק��蹳�����ҡC
```
$ sudo docker tag 5db5f8471261 ouruser/sinatra:devel
$ sudo docker images ouruser/sinatra
REPOSITORY          TAG     IMAGE ID      CREATED        VIRTUAL SIZE
ouruser/sinatra     latest  5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     devel   5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     v2      5db5f8471261  11 hours ago   446.7 MB
```

*���G��h�Ϊk�A�аѦ� [Dockerfile](../dockerfile/README.md) ���`�C

### �q���a���t�ξɤJ
�n�q���a���t�ξɤJ�@���蹳�A�i�H�ϥ� openvz�]�e�������ƪ����W�޳N�^���ҪO�ӳЫءG
openvz ���ҪO�U���a�}�� [templates](http://openvz.org/Download/templates/precreated) �C

��p�A���U���F�@�� ubuntu-14.04 ���蹳�A����ϥΥH�U�R�O�ɤJ�G
```
sudo cat ubuntu-14.04-x86_64-minimal.tar.gz  |docker import - ubuntu:14.04
```
�M��d�ݷs�ɤJ���蹳�C
```
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              14.04               05ac7c0b9383        17 seconds ago      215.5 MB
```

###�W���蹳
�Τ�i�H�q�L `docker push` �R�O�A��ۤv�Ыت��蹳�W�Ǩ�ܮw���Ӧ@�ɡC�Ҧp�A�Τ�b Docker Hub �W�������U��A�i�H���e�ۤv���蹳��ܮw���C
```
$ sudo docker push ouruser/sinatra
The push refers to a repository [ouruser/sinatra] (len: 1)
Sending image list
Pushing repository ouruser/sinatra (3 tags)
```
