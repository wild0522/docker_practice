## �ܨҡG�Ыؤ@���I���I�s��
�q�{���p�U�ADocker �|�N�Ҧ��e���s����� `docker0` ���Ѫ������l�����C

�Τᦳ�ɭԻݭn��Ӯe�������i�H���s�q�H�A�Ӥ��γq�L�D�������i������C

�ѨM��k��²��G�Ыؤ@�� `peer` ���f�A���O����Ӯe�����A�t�m���I���I��������Y�i�C

�����Ұ� 2 �Ӯe���G
```
$ sudo docker run -i -t --rm --net=none base /bin/bash
root@1f1f4c1f931a:/#
$ sudo docker run -i -t --rm --net=none base /bin/bash
root@12e343489d2f:/#
```

���i�{���A�M��Ыغ����W�r�Ŷ������ܤ��C
```
$ sudo docker inspect -f '{{.State.Pid}}' 1f1f4c1f931a
2989
$ sudo docker inspect -f '{{.State.Pid}}' 12e343489d2f
3004
$ sudo mkdir -p /var/run/netns
$ sudo ln -s /proc/2989/ns/net /var/run/netns/2989
$ sudo ln -s /proc/3004/ns/net /var/run/netns/3004
```

�Ыؤ@�� `peer` ���f�A�M��t�m����
```
$ sudo ip link add A type veth peer name B

$ sudo ip link set A netns 2989
$ sudo ip netns exec 2989 ip addr add 10.1.1.1/32 dev A
$ sudo ip netns exec 2989 ip link set A up
$ sudo ip netns exec 2989 ip route add 10.1.1.2/32 dev A

$ sudo ip link set B netns 3004
$ sudo ip netns exec 3004 ip addr add 10.1.1.2/32 dev B
$ sudo ip netns exec 3004 ip link set B up
$ sudo ip netns exec 3004 ip route add 10.1.1.1/32 dev B
```
�{�b�o 2 �Ӯe���N�i�H�ۤ� ping �q�A�æ��\�إ߳s���C�I���I������ݭn�l���M�l�����X�C

���~�A�]�i�H�����w `--net=none` �ӳЫ��I���I����C�o�ˮe���٥i�H�q�L����������ӳq�H�C

�Q����������k�A�i�H�Ыؤ@�ӥu��D���q�H���e���C���O�@�뱡�p�U�A����˨ϥ� `--icc=false` �������e���������q�H�C
