## Swarm �L�o��
swarm ���ի׾�(scheduler)�b��ܸ`�I�B��e�����ɭԤ���X�عL�o�� (filter)�GConstraint,Affinity,Port,Dependency,Health

�i�H�b���� `swarm manage` �R�O���ɭԳq�L `--filter` �ﶵ�ӳ]�m�C

###Constraint Filter
constraint �O�@�Ӹ����`�I�����p����ȹ�A�i�H�ݰ��O�C�Ӹ`�I�����ҡA�o�Ӽ��ҥi�H�b�Ұ�docker daemon���ɭԫ��w�A��p
```sh
sudo docker -d --label label_name=label01
```

�]�i�H�g�bdocker���t�m���̭��]�bubuntu�W���O `/etc/default/docker`�^�C

�b�������礤�A��083�K�[����--label label_name=083,084�K�[����--label label_name=084,124�K�[����--label label_name=084,

�H083���ҡA���}/etc/default/docker���A�ק�DOCKER_OPTS�G
```sh
DOCKER_OPTS="-H 0.0.0.0:2375 -H unix:///var/run/docker.sock  --label label_name=083"
```

�b�ϥ�docker run�R�O�Ұʮe�����ɭԨϥ� `-e constarint:key=value` ���Φ��A�i�H���wcontainer�B�檺�`�I�C

��p�ڭ̷Q�b84�W���Ұʤ@�� redis �e���C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_1 -d -e constraint:label_name==084 redis
fee1b7b9dde13d64690344c1f1a4c3f5556835be46b41b969e4090a083a6382d
```
�`�N�A�O**���**�����A���O�@�ӵ����A�o�@�I�|�g�`�Q�����C

���U�ӦA�b084�o�x�����W�Ұʤ@��redis �e���C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_2 -d -e constraint:label_name==084 redis         4968d617d9cd122fc2e17b3bad2f2c3b5812c0f6f51898024a96c4839fa000e1
```
�M��A�b083�o�x�����W�Ұʥt�~�@�� redis �e���C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_3 -d -e constraint:label_name==083 redis         7786300b8d2232c2335ac6161c715de23f9179d30eb5c7e9c4f920a4f1d39570
```

�{�b�ӬݤU���污�p�G
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
7786300b8d22        redis:latest        "/entrypoint.sh redi   15 minutes ago      Up 53 seconds       6379/tcp            083/redis_3
4968d617d9cd        redis:latest        "/entrypoint.sh redi   16 minutes ago      Up 2 minutes        6379/tcp            084/redis_2
fee1b7b9dde1        redis:latest        "/entrypoint.sh redi   19 minutes ago      Up 5 minutes        6379/tcp            084/redis_1
```

�i�H�ݨ�A���浲�G��w�����@�ˡC

���O�p�G���w�@�Ӥ��s�b�����Ҫ��ܨӹB��e���|�����C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name redis_0 -d -e constraint:label_name==0 redis
FATA[0000] Error response from daemon: unable to find a node that satisfies label_name==0
```

###Affinity Filter
�q�L�ϥ� Affinity Filter�A�i�H���@�Ӯe������ۥt�@�Ӯe���ҰʡA�]�N�O������Ӯe���b�P�@�Ӹ`�I�W���ҰʡC

�{�b�䤤�@�x�����W���Ұʤ@�� redis �e���C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run -d --name redis redis
ea13eddf667992c5d8296557d3c282dd8484bd262c81e2b5af061cdd6c82158d
rio@085:~$ sudo docker -H 192.168.1.83:2376  ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS               NAMES
ea13eddf6679        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   6379/tcp            083/redis
```

�M��A���Ұʨ�� redis �e���C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376  run -d --name redis_1 -e affinity:container==redis redis
bac50c2e955211047a745008fd1086eaa16d7ae4f33c192f50412e8dcd0a14cd
rio@085:~$ sudo docker -H 192.168.1.83:2376  run -d --name redis_1 -e affinity:container==redis redis
bac50c2e955211047a745008fd1086eaa16d7ae4f33c192f50412e8dcd0a14cd
```
�{�b�Ӭd�ݤU�B�浲�G,�i�H�ݨ�T�Ӯe�����O�b�@�x�����W�B��
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376  ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS               NAMES
449ed25ad239        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   6379/tcp            083/redis_2
bac50c2e9552        redis:latest        /entrypoint.sh redis   25 minutes ago      Up 10 seconds           6379/tcp            083/redis_1
ea13eddf6679        redis:latest        /entrypoint.sh redis   28 minutes ago      Up 3 minutes            6379/tcp            083/redis
```
�q�L `-e affinity:image=image_name` �R�O�i�H���w�u���w�g�U���F`image_name`�蹳�������~�B��e��
```sh
sudo docker �VH 192.168.1.83:2376 run �Vname redis1 �Vd �Ve affinity:image==redis redis 
```
redis1 �o�Ӯe���u�|�b�w�g�U���F redis �蹳���`�I�W�B��C

```sh
sudo docker -H 192.168.1.83:2376 run -d --name redis -e affinity:image==~redis redis
```
�o���R�O�F�쪺�ĪG�O�G�b�� redis �蹳���`�I�W���Ұʤ@�ӦW�r�s�� redis ���e���A�p�G�C�Ӹ`�I�W�����S�� redis �e���A�N�����q�{�������Ұ� redis �e���C

###Port Filter
Port �]�|�Q�{���O�@�Ӱߤ@���귽
```sh
sudo docker -H 192.168.1.83:2376 run -d -p 80:80 nginx
```

���槹�o���R�O�A�������ϥ� 80 �ݤf���e�����O�Ұʥ��ѡC
