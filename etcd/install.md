## �w��

etcd ��� Go �y����{�A�]���A�Τ�i�H�q [���إD��](https://github.com/coreos/etcd) �U�����N�X�ۦ�sĶ�A�]�i�H�U���sĶ�n���G�i����A�Ʀܪ����ϥλs�@�n�� Docker �蹳��������C

### �G�i����覡�U��

�sĶ�n���G�i���󳣦b [github.com/coreos/etcd/releases](https://github.com/coreos/etcd/releases/) �����A�Τ�i�H��ܻݭn�������A�γq�L�U���u��U���C

�Ҧp�A�U�����R�O�ϥ� curl �u��U�����Y�]�A�ø����C

```
curl -L  https://github.com/coreos/etcd/releases/download/v2.0.0-rc.1/etcd-v2.0.0-rc.1-linux-amd64.tar.gz -o etcd-v2.0.0-rc.1-linux-amd64.tar.gz
tar xzvf etcd-v2.0.0-rc.1-linux-amd64.tar.gz
cd etcd-v2.0.0-rc.1-linux-amd64
```

������A�i�H�ݨ���]�A
```
$ ls
etcd  etcdctl  etcd-migrate  README-etcdctl.md  README.md
```

�䤤 etcd �O�A�ȥD���Aetcdctl �O���ѵ��Τ᪺�R�O�Ȥ�ݡAetcd-migrate �t�d�i��E���C

���˳q�L�U�����R�O�N�T�Ӥ�󳣩��t�Υi����ؿ� `/usr/local/bin/` �� `/usr/bin/`�C

```
$ sudo cp etcd* /usr/local/bin/
```

�B�� etcd�A�N�q�{�իؤ@�Ө�Ӹ`�I�����s�C�ƾڮw�A�Ⱥ��q�{��ť�b 2379 �M 4001 �ݤf�Aetcd ��Һ�ť�b 2380 �M 7001 �ݤf�C��������p�U���H���G
```
$ ./etcd
2014/12/31 14:52:09 no data-dir provided, using default data-dir ./default.etcd
2014/12/31 14:52:09 etcd: listening for peers on http://localhost:2380
2014/12/31 14:52:09 etcd: listening for peers on http://localhost:7001
2014/12/31 14:52:09 etcd: listening for client requests on http://localhost:2379
2014/12/31 14:52:09 etcd: listening for client requests on http://localhost:4001
2014/12/31 14:52:09 etcdserver: name = default
2014/12/31 14:52:09 etcdserver: data dir = default.etcd
2014/12/31 14:52:09 etcdserver: snapshot count = 10000
2014/12/31 14:52:09 etcdserver: advertise client URLs = http://localhost:2379,http://localhost:4001
2014/12/31 14:52:09 etcdserver: initial advertise peer URLs = http://localhost:2380,http://localhost:7001
2014/12/31 14:52:09 etcdserver: initial cluster = default=http://localhost:2380,default=http://localhost:7001
2014/12/31 14:52:10 etcdserver: start member ce2a822cea30bfca in cluster 7e27652122e8b2ae
2014/12/31 14:52:10 raft: ce2a822cea30bfca became follower at term 0
2014/12/31 14:52:10 raft: newRaft ce2a822cea30bfca [peers: [], term: 0, commit: 0, lastindex: 0, lastterm: 0]
2014/12/31 14:52:10 raft: ce2a822cea30bfca became follower at term 1
2014/12/31 14:52:10 etcdserver: added local member ce2a822cea30bfca [http://localhost:2380 http://localhost:7001] to cluster 7e27652122e8b2ae
2014/12/31 14:52:11 raft: ce2a822cea30bfca is starting a new election at term 1
2014/12/31 14:52:11 raft: ce2a822cea30bfca became candidate at term 2
2014/12/31 14:52:11 raft: ce2a822cea30bfca received vote from ce2a822cea30bfca at term 2
2014/12/31 14:52:11 raft: ce2a822cea30bfca became leader at term 2
2014/12/31 14:52:11 raft.node: ce2a822cea30bfca elected leader ce2a822cea30bfca at term 2
2014/12/31 14:52:11 etcdserver: published {Name:default ClientURLs:[http://localhost:2379 http://localhost:4001]} to cluster 7e27652122e8b2ae
```

���ɡA�i�H�ϥ� etcdctl �R�O�i����աA�]�m�M������ `testkey: "hello world"`�A�ˬd etcd �A�ȬO�_�Ұʦ��\�G
```
$ ./etcdctl set testkey "hello world"
hello world
$ ./etcdctl get testkey
hello world
```
���� etcd �A�Ȥw�g���\�ҰʤF�C

��M�A�]�i�H�q�L HTTP �X�ݥ��a 2379 �� 4001 �ݤf���覡�Ӷi��ާ@�A�Ҧp�d�� `testkey` ���ȡG
```
$ curl -L http://localhost:4001/v2/keys/testkey
{"action":"get","node":{"key":"/testkey","value":"hello world","modifiedIndex":3,"createdIndex":3}}
```

### Docker �蹳�覡�U��

�蹳�W�٬� quay.io/coreos/etcd:v2.0.0_rc.1�A�i�H�q�L�U�����R�O�Ұ� etcd �A�Ⱥ�ť�� 4001 �ݤf�C
```
$ sudo docker run -p 4001:4001 -v /etc/ssl/certs/:/etc/ssl/certs/ quay.io/coreos/etcd:v2.0.0_rc.1
```
