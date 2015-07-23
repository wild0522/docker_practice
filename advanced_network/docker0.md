## �t�m docker0 ����
Docker �A���q�{�|�Ыؤ@�� `docker0` �����]��W���@�� `docker0` �������f�^�A���b���ּh�s�q�F��L�����z�ε������d�A�o�N�N�Ҧ��e���M���a�D�������P�@�Ӫ��z�����C

Docker �q�{���w�F `docker0` ���f �� IP �a�}�M�l�����X�A���D���M�e�������i�H�q�L�����ۤ��q�H�A���ٵ��X�F MTU�]���f���\�������̤j�ǿ�椸�^�A�q�`�O 1500 Bytes�A�αJ�D�D���������ѤW������q�{�ȡC�o�ǭȳ��i�H�b�A�ȱҰʪ��ɭԶi��t�m�C
* `--bip=CIDR` -- IP �a�}�[���X�榡�A�Ҧp 192.168.1.5/24
* `--mtu=BYTES` -- �л\�q�{�� Docker mtu �t�m

�]�i�H�b�t�m��󤤰t�m DOCKER_OPTS�A�M�᭫�ҪA�ȡC
�ѩ�ثe Docker �����O Linux �����A�Τ�i�H�ϥ� `brctl show` �Ӭd�ݺ����M�ݤf�s���H���C
```
$ sudo brctl show
bridge name     bridge id               STP enabled     interfaces
docker0         8000.3a1d7362b4ee       no              veth65f9
                                             vethdda6
```
*���G`brctl` �R�O�b Debian�BUbuntu ���i�H�ϥ� `sudo apt-get install bridge-utils` �Ӧw�ˡC


�C���Ыؤ@�ӷs�e�����ɭԡADocker �q�i�Ϊ��a�}�q����ܤ@�ӪŶ��� IP �a�}���t���e���� eth0 �ݤf�C�ϥΥ��a�D���W `docker0` ���f�� IP �@���Ҧ��e�����q�{�����C
```
$ sudo docker run -i -t --rm base /bin/bash
$ ip addr show eth0
24: eth0: <BROADCAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 32:6f:e0:35:57:91 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::306f:e0ff:fe35:5791/64 scope link
       valid_lft forever preferred_lft forever
$ ip route
default via 172.17.42.1 dev eth0
172.17.0.0/16 dev eth0  proto kernel  scope link  src 172.17.0.3
$ exit
```
