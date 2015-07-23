## �e���X�ݱ���
�e�����X�ݱ���A�D�n�q�L Linux �W�� `iptables` ������Ӷi��޲z�M��{�C`iptables` �O Linux �W�q�{��������n��A�b�j�����o�檩�����۱a�C

### �e���X�ݥ~������
�e���n�Q�X�ݥ~�������A�ݭn���a�t�Ϊ���o����C�bLinux �t�Τ��A�ˬd��o�O�_���}�C

```
$sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1
```
�p�G�� 0�A�����S���}����o�A�h�ݭn��ʥ��}�C
```
$sysctl -w net.ipv4.ip_forward=1
```
�p�G�b�Ұ� Docker �A�Ȫ��ɭԳ]�w `--ip-forward=true`, Docker �N�|�۰ʳ]�w�t�Ϊ� `ip_forward` �ѼƬ� 1�C

### �e�������X��
�e�������ۤ��X�ݡA�ݭn��譱������C
* �e���������ݼ��O�_�w�g���p�C�q�{���p�U�A�Ҧ��e�����|�Q�s���� `docker0` �����W�C
* ���a�t�Ϊ�������n�� -- `iptables` �O�_���\�q�L�C

#### �X�ݩҦ��ݤf
��Ұ� Docker �A�ȮɭԡA�q�{�|�K�[�@����o������ iptables �� FORWARD ��W�C�������q�L�]`ACCEPT`�^�٬O�T��]`DROP`�^���M��t�m`--icc=true`�]�ʬ٭ȡ^�٬O `--icc=false`�C��M�A�p�G��ʫ��w `--iptables=false` �h���|�K�[ `iptables` �W�h�C

�i���A�q�{���p�U�A���P�e�������O���\�������q���C�p�G���F�w���Ҽ{�A�i�H�b `/etc/default/docker` ��󤤰t�m `DOCKER_OPTS=--icc=false` �ӸT��C

#### �X�ݫ��w�ݤf
�b�q�L `-icc=false` ���������X�ݫ�A�٥i�H�q�L `--link=CONTAINER_NAME:ALIAS` �ﶵ�ӳX�ݮe�����}��ݤf�C

�Ҧp�A�b�Ұ� Docker �A�ȮɡA�i�H�P�ɨϥ� `icc=false --iptables=true` �Ѽƨ��������\�ۤ��������X�ݡA���� Docker �i�H�ק�t�Τ��� `iptables` �W�h�C

���ɡA�t�Τ��� `iptables` �W�h�i��O����
```
$ sudo iptables -nL
...
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
DROP       all  --  0.0.0.0/0            0.0.0.0/0
...
```

����A�Ұʮe���]`docker run`�^�ɨϥ� `--link=CONTAINER_NAME:ALIAS` �ﶵ�CDocker �|�b `iptable` ���� ��Ӯe�����O�K�[�@�� `ACCEPT` �W�h�A���\�ۤ��X�ݶ}�񪺺ݤf�]���M�� Dockerfile ���� EXPOSE ��^�C

��K�[�F `--link=CONTAINER_NAME:ALIAS` �ﶵ��A�K�[�F `iptables` �W�h�C
```
$ sudo iptables -nL
...
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  172.17.0.2           172.17.0.3           tcp spt:80
ACCEPT     tcp  --  172.17.0.3           172.17.0.2           tcp dpt:80
DROP       all  --  0.0.0.0/0            0.0.0.0/0
```

�`�N�G`--link=CONTAINER_NAME:ALIAS` ���� `CONTAINER_NAME` �ثe�����O Docker ���t���W�r�A�Ψϥ� `--name` �Ѽƫ��w���W�r�C�D���W�h���|�Q�ѧO�C
