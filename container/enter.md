## �i�J�e��
�b�ϥ� `-d` �ѼƮɡA�e���Ұʫ�|�i�J��x�C
�Y�ǮɭԻݭn�i�J�e���i��ާ@�A���ܦh�ؤ�k�A�]�A�ϥ� `docker attach` �R�O�� `nsenter` �u�㵥�C
### attach �R�O
`docker attach` �ODocker�۱a���R�O�C�U���ܨҦp��ϥθөR�O�C
```
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$sudo docker attach nostalgic_hypatia
root@243c32535da7:/#
```
���O�ϥ� `attach` �R�O���ɭԨä���K�C��h�ӵ��f�P�� attach ��P�@�Ӯe�����ɭԡA�Ҧ����f���|�P�B��ܡC��Y�ӵ��f�]�R�O�����,��L���f�]�L�k����ާ@�F�C

### nsenter �R�O
#### �w��
`nsenter` �u��b util-linux �]2.23������]�t�C
�p�G�t�Τ� util-linux �]�S���өR�O�A�i�H���ӤU������k�q���X�w�ˡC
```
$ cd /tmp; curl https://www.kernel.org/pub/linux/utils/util-linux/v2.24/util-linux-2.24.tar.gz | tar -zxf-; cd util-linux-2.24;
$ ./configure --without-ncurses
$ make nsenter && sudo cp nsenter /usr/local/bin
```

#### �ϥ�
`nsenter` �i�H�X�ݥt�@�Ӷi�{���W�r�Ŷ��Cnsenter �n���`�u�@�ݭn�� root �v���C
�ܤ����AUbuntu 14.04 ���M�ϥΪ��O util-linux 2.20�C�w�˳̷s������ util-linux�]2.24�^���A�Ы��ӥH�U�B�J�G
```
$ wget https://www.kernel.org/pub/linux/utils/util-linux/v2.24/util-linux-2.24.tar.gz; tar xzvf util-linux-2.24.tar.gz
$ cd util-linux-2.24
$ ./configure --without-ncurses && make nsenter
$ sudo cp nsenter /usr/local/bin
```
���F�s����e���A�A�ٻݭn���e�����Ĥ@�Ӷi�{�� PID�A�i�H�q�L�U�����R�O����C
```
PID=$(docker inspect --format "{{ .State.Pid }}" <container>)
```
�q�L�o�� PID�A�N�i�H�s����o�Ӯe���G
```
$ nsenter --target $PID --mount --uts --ipc --net --pid
```
�U�����X�@�ӧ��㪺�Ҥl�C
```
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$ PID=$(docker-pid 243c32535da7)
10981
$ sudo nsenter --target 10981 --mount --uts --ipc --net --pid
root@243c32535da7:/#
```
��²�檺�A��ĳ�j�a�U��
[.bashrc_docker](https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker)�A�ñN���e��� .bashrc ���C
```
$ wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
$ echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc
```
�o�Ӥ�󤤩w�q�F�ܦh��K�ϥ� Docker ���R�O�A�Ҧp `docker-pid` �i�H����Y�Ӯe���� PID�F�� `docker-enter` �i�H�i�J�e���Ϊ����b�e��������R�O�C
```
$ echo $(docker-pid <container>)
$ docker-enter <container> ls
```
