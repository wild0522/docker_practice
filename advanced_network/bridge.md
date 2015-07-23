## �۩w�q����
���F�q�{�� `docker0` �����A�Τ�]�i�H���w�����ӳs���U�Ӯe���C

�b�Ұ� Docker �A�Ȫ��ɭԡA�ϥ� `-b BRIDGE`��`--bridge=BRIDGE` �ӫ��w�ϥΪ������C

�p�G�A�Ȥw�g�B��A���ݭn������A�ȡA�çR���ª������C
```
$ sudo service docker stop
$ sudo ip link set dev docker0 down
$ sudo brctl delbr docker0
```
�M��Ыؤ@�Ӻ��� `bridge0`�C
```
$ sudo brctl addbr bridge0
$ sudo ip addr add 192.168.5.1/24 dev bridge0
$ sudo ip link set dev bridge0 up
```
�d�ݽT�{�����ЫبñҰʡC
```
$ ip addr show bridge0
4: bridge0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state UP group default
    link/ether 66:38:d0:0d:76:18 brd ff:ff:ff:ff:ff:ff
    inet 192.168.5.1/24 scope global bridge0
       valid_lft forever preferred_lft forever
```
�t�m Docker �A�ȡA�q�{������Ыت������W�C
```
$ echo 'DOCKER_OPTS="-b=bridge0"' >> /etc/default/docker
$ sudo service docker start
```
�Ұ� Docker �A�ȡC
�s�ؤ@�Ӯe���A�i�H�ݨ쥦�w�g������F `bridge0` �W�C

�i�H�~��� `brctl show` �R�O�d�ݾ������H���C�t�~�A�b�e�����i�H�ϥ� `ip addr` �M `ip route` �R�O�Ӭd�� IP �a�}�t�m�M���ѫH���C
