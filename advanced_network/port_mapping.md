## �M�g�e���ݤf��J�D�D������{

�q�{���p�U�A�e���i�H�D�ʳX�ݨ�~���������s���A���O�~�������L�k�X�ݨ�e���C
### �e���X�ݥ~����{
�e���Ҧ���~���������s���A���a�}���|�QNAT�����a�t�Ϊ�IP�a�}�C�o�O�ϥ� `iptables` �����a�}���˾ާ@��{���C

�d�ݥD���� NAT �W�h�C
```
$ sudo iptables -t nat -nL
...
Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
MASQUERADE  all  --  172.17.0.0/16       !172.17.0.0/16
...
```
�䤤�A�W�z�W�h�N�Ҧ����a�}�b `172.17.0.0/16` ���q�A�ؼЦa�}����L���q�]�~�������^���y�q�ʺA���ˬ��q�t�κ��d�o�X�CMASQUERADE ��ǲ� SNAT ���n�B�O����ʺA�q���d����a�}�C

### �~���X�ݮe����{

�e�����\�~���X�ݡA�i�H�b `docker run` �ɭԳq�L `-p` �� `-P` �ѼƨӱҥΡC

���ޥΨ��ؿ�k�A���]�O�b���a�� `iptable` �� nat ���K�[�������W�h�C

�ϥ� `-P` �ɡG
```
$ iptables -t nat -nL
...
Chain DOCKER (2 references)
target     prot opt source               destination
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:49153 to:172.17.0.2:80
```

�ϥ� `-p 80:80` �ɡG
```
$ iptables -t nat -nL
Chain DOCKER (2 references)
target     prot opt source               destination
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80 to:172.17.0.2:80
```
�`�N�G
* �o�̪��W�h�M�g�F 0.0.0.0�A�N���۱N�����D���Ӧ۩Ҧ����f���y�q�C�Τ�i�H�q�L `-p IP:host_port:container_port` �� `-p
IP::port` �ӫ��w���\�X�ݮe�����D���W�� IP�B���f���A�H��w���Y�檺�W�h�C
* �p�G�Ʊ�ä[�j�w��Y�өT�w�� IP �a�}�A�i�H�b Docker �t�m��� `/etc/default/docker` �����w `DOCKER_OPTS="--ip=IP_ADDRESS"`�A���᭫�� Docker �A�ȧY�i�ͮġC
