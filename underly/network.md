## Docker ������{

Docker ��������{���N�O�Q�ΤF Linux �W�������W�r�Ŷ��M���������]�ơ]�S�O�O veth pair�^�C��ĳ�����x�A�ѳo�ⳡ�����򥻷����A�\Ū�����C

### �򥻭�z
�����A�n��{�����q�H�A�����ݭn�ܤ֤@�Ӻ������f�]���z���f�ε������f�^�Ӧ��o�ƾڥ]�F���~�A�p�G���P�l�������n�i��q�H�A�ݭn���Ѿ���C

Docker �����������f�q�{���O���������f�C�������f���u�դ��@�O��o�Ĳv�����C
Linux �q�L�b���֤��i��ƾڽƻs�ӹ�{�������f�������ƾ���o�A�o�e���f���o�e�w�s�����ƾڥ]�Q�����ƻs�챵�����f�������w�s���C��󥻦a�t�ΩM�e�����t�άݨӴN���O�@�ӥ��`���H�Ӻ��d�A�u�O�����ݭn�u���P�~�������]�Ƴq�H�A�t�׭n�֫ܦh�C

Docker �e�������N�Q�ΤF�o���޳N�C���b���a�D���M�e�������O�Ыؤ@�ӵ������f�A�������̩����s�q�]�o�˪��@�ﱵ�f�s�� `veth pair`�^�C

### �Ыغ����Ѽ�
Docker �Ыؤ@�Ӯe�����ɭԡA�|����p�U�ާ@�G
* �Ыؤ@��������f�A���O��쥻�a�D���M�s�e�����F
* ���a�D���@�ݾ������q�{�� docker0 �Ϋ��w�����W�A�è㦳�@�Ӱߤ@���W�r�A�p veth65f9�F
* �e���@�ݩ��s�e�����A�íק�W�r�@�� eth0�A�o�ӱ��f�u�b�e�����W�r�Ŷ��i���F
* �q�����i�Φa�}�q������@�ӪŶ��a�}���t���e���� eth0�A�ðt�m�q�{���Ѩ�������d veth65f9�C

�����o�Ǥ���A�e���N�i�H�ϥ� eth0 �������d�ӳs����L�e���M��L�����C

�i�H�b `docker run` ���ɭԳq�L `--net` �Ѽƨӫ��w�e���������t�m�A��4�ӥi��ȡG
* `--net=bridge` �o�ӬO�q�{�ȡA�s�����q�{�������C
* `--net=host` �i�D Docker ���n�N�e���������j�����W�r�Ŷ����A�Y���n�e���Ʈe�����������C���ɮe���ϥΥ��a�D���������A���֦����������a�D�����f�X���v���C�e���i�{�i�H��D���䥦 root �i�{�@�˥i�H���}�C�d�򪺺ݤf�A�i�H�X�ݥ��a�����A�Ȥ�p D-bus�A�٥i�H���e�����@�Ǽv�T��ӥD���t�Ϊ��Ʊ��A��p���ҥD���C�]���ϥγo�ӿﶵ���ɭԭn�D�`�p�ߡC�p�G�i�@�B���ϥ� `--privileged=true`�A�e���|�Q���\�����t�m�D����������̡C
* `--net=container:NAME_or_ID` �� Docker �N�s�خe�����i�{���@�Ӥw�s�b�e���������̤��A�s�e���i�{���ۤv�����t�ΡB�i�{�C��M�귽����A���|�M�w�s�b���e���@�� IP �a�}�M�ݤf�������귽�A��̶i�{�i�H�����q�L `lo` ���^���f�q�H�C
* `--net=none` �� Docker �N�s�e�����j���������̤��A���O���i������t�m�C����A�Τ�i�H�ۤv�i��t�m�C

### �����t�m�Ӹ`
�Τ�ϥ� `--net=none` ��A�i�H�ۦ�t�m�����A���e���F��򥭱`�@�˨㦳�X�ݺ������v���C�q�L�o�ӹL�{�A�i�H�A�� Docker �t�m�������Ӹ`�C

�����A�Ұʤ@�� `/bin/bash` �e���A���w `--net=none` �ѼơC
```
$ sudo docker run -i -t --rm --net=none base /bin/bash
root@63f36fc01b5f:/#
```
�b���a�D���d��e�����i�{ id�A�ì����Ыغ����R�W�Ŷ��C
```
$ sudo docker inspect -f '{{.State.Pid}}' 63f36fc01b5f
2778
$ pid=2778
$ sudo mkdir -p /var/run/netns
$ sudo ln -s /proc/$pid/ns/net /var/run/netns/$pid
```
�ˬd�������d�� IP �M�l�����X�H���C
```
$ ip addr show docker0
21: docker0: ...
inet 172.17.42.1/16 scope global docker0
...
```
�Ыؤ@�� �uveth pair�v ���f A �M B�A�j�w A ����� `docker0`�A�ñҥΥ�
```
$ sudo ip link add A type veth peer name B
$ sudo brctl addif docker0 A
$ sudo ip link set A up
```
�NB���e���������R�W�Ŷ��A�R�W�� eth0�A�Ұʥ��ðt�m�@�ӥi�� IP�]�������q�^�M�q�{�����C
```
$ sudo ip link set B netns $pid
$ sudo ip netns exec $pid ip link set dev B name eth0
$ sudo ip netns exec $pid ip link set eth0 up
$ sudo ip netns exec $pid ip addr add 172.17.42.99/16 dev eth0
$ sudo ip netns exec $pid ip route add default via 172.17.42.1
```
�H�W�A�N�O Docker �t�m����������L�{�C

��e��������ADocker �|�M�Ůe���A�e������ eth0 �|�H�����R�W�Ŷ��@�_�Q�M���AA ���f�]�Q�۰ʱq `docker0` �����C

���~�A�Τ�i�H�ϥ� `ip netns exec` �R�O�Ӧb���w�����W�r�Ŷ����i��t�m�A�q�Ӱt�m�e�����������C
