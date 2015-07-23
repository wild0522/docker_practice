## �h�x���z�D���������e�����p�]���S�e����u��������^
Docker �q�{���������d�O docker0�C���u�|�b���������Ҧ����e�����d�A�|�Ҩӻ��e�����������d�b�D���W�ݤ@��s�� veth***  �� Docker �u�O��Ҧ��o�Ǻ��d�����b�@�_�A�p�U�G
```
[root@opnvz ~]# brctl show
bridge name     bridge id               STP enabled     interfaces
docker0         8000.56847afe9799       no              veth0889
                                             veth3c7b
                                             veth4061
```
�b�e�����ݨ쪺�a�}�@��O���U���o�˪��a�}�G
```
root@ac6474aeb31d:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
11: eth0: <BROADCAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 4a:7d:68:da:09:cf brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::487d:68ff:feda:9cf/64 scope link
       valid_lft forever preferred_lft forever
```
�o�˴N�i�H��o�Ӻ����ݦ��O�@�Өp���������A�q�L nat �s���~���A�p�G�n���~���s����e�����A�N�ݭn���ݤf�M�g�A�Y -p �ѼơC

�p�G�b���~�������ΡA�Ϊ̰��h�Ӫ��z�D�������s�A�i��ݭn�N�h�Ӫ��z�D�����e���ը�@�Ӫ��z�������ӡA����N�ݭn�N�o�Ӻ���������ڭ̫��w�����d�W�C

### �ݼ���
�D�� A �M�D�� B �����d�@���s�۪��z�洫�����P�@�� vlan 101,�o�˺����@�M�����T�N�۷��b�P�@�Ӫ��z�������F�A�Ӯe���@�B�e���T�B�e���|�]�b�P�@���z�������F�A�L�̤����i�H�ۤ��q�H�A�ӥB�i�H��P�@ vlan ������L���z�������p�C
![���z�ݼ���](../_images/container_connect_topology.png)

### ubuntu �ܨ�
�U���H ubuntu ���ҳЫئh�ӥD�����e���p��:
�Ыئۤv������,�s�� /etc/network/interface ���
```
auto br0
iface br0 inet static
address 192.168.7.31
netmask 255.255.240.0
gateway 192.168.7.254
bridge_ports em1
bridge_stp off
dns-nameservers 8.8.8.8 192.168.6.1
```
�N Docker ���q�{�����j�w��o�ӷs�ت� br0 �W���A�o�˴N�N�o�x�����W�e���j�w�� em1 �o�Ӻ��d�ҹ��������z�����W�F�C

ubuntu �ק� /etc/default/docker ���A�K�[�̫�@�椺�e

```
# Docker Upstart and SysVinit configuration file
# Customize location of Docker binary (especially for development testing).
#DOCKER="/usr/local/bin/docker"
# Use DOCKER_OPTS to modify the daemon startup options.
#DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"

# If you need Docker to use an HTTP proxy, it can also be specified here.
#export http_proxy="http://127.0.0.1:3128/"

# This is also a handy place to tweak where Docker's temporary files go.
#export TMPDIR="/mnt/bigdrive/docker-tmp"

DOCKER_OPTS="-b=br0"
```

�b�Ұ� Docker ���ɭ� �ϥ� -b �Ѽ� �N�e���j�w�쪫�z�����W�C���� Docker �A�ȫ�A�A�i�J�e���i�H�ݨ쥦�w�g�j�w��A�����z�����W�F�C

```
root@ubuntudocker:~# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                        NAMES
58b043aa05eb        desk_hz:v1          "/startup.sh"       5 days ago          Up 2 seconds        5900/tcp, 6080/tcp, 22/tcp   yanlx
root@ubuntudocker:~# brctl show
bridge name     bridge id               STP enabled     interfaces
br0             8000.7e6e617c8d53       no              em1
                                            vethe6e5
```
�o�˴N������e�����S�쪫�z�����W�F�A�h�x���z�D�����e���]�i�H�ۤ��p���F�C�ݭn�`�N���O�A�o�˴N�ݭn�ۤv�ӫO�Үe���������w���F�C
