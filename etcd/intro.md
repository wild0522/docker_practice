## ����O etcd

![](../_images/etcd_logo.png)

etcd �O CoreOS �ζ��� 2013 �~ 6 ��o�_���}�����ءA�����ؼЬO�c�ؤ@�Ӱ��i�Ϊ����G����ȡ]key-value�^�ƾڮw�A��� Go �y����{�C�ڭ̪��D�A�b���G���t�Τ��A�U�تA�Ȫ��t�m�H�����޲z���ɡA�A�Ȫ��o�{�O�@�ӫܰ򥻦P�ɤ]�O�ܭ��n�����D�CCoreOS ���شN�Ʊ��� etcd �ӸѨM�o�@���D�C

etcd �ثe�b [github.com/coreos/etcd](https://github.com/coreos/etcd) �i����@�A�Y�N�o�G 2.0.0 �����C

���� [Apache ZooKeeper](http://zookeeper.apache.org/) ���ةM [doozer](https://github.com/ha/doozerd) ���ت��ҵo�Aetcd �b�]�p���ɭԭ��I�Ҽ{�F�U���|�ӭn���G
* ²��G��� REST ���檺 HTTP+JSON API
* �w���G��� HTTPS �覡���X��
* �ֳt�G����õo 1k/s ���g�ާ@
* �i�a�G������G�����c�A��� Raft ���@�P�ʺ�k

*���GApache ZooKeeper �O�@�M���W�����G���t�Τ��i��P�B�M�@�P�ʺ޲z���u��C*
*���Gdoozer �h�O�@�Ӥ@�P�ʤ��G���ƾڮw�C*
*���GRaft �O�@�M�q�L���|�D�`�I�ӹ�{���G���t�Τ@�P�ʪ���k�A�ۤ��j�W������ Paxos ��k�A�����L�{��e���Q�H�z�ѡA�� Stanford �j�Ǫ� Diego Ongaro �M John Ousterhout ���X�C��h�Ӹ`�i�H�Ѧ� [raftconsensus.github.io](http://raftconsensus.github.io)�C*

�@�뱡�p�U�A�Τ�ϥ� etcd �i�H�b�h�Ӹ`�I�W�Ұʦh�ӹ�ҡA�òK�[���̬��@�Ӷ��s�C�P�@�Ӷ��s���� etcd ��ұN�|�O�������H�����@�P�ʡC
