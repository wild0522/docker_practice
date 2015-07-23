## �ϥ� etcdctl

etcdctl �O�@�өR�O��Ȥ�ݡA���ണ�Ѥ@��²�䪺�R�O�A�ѥΤ᪽���� etcd �A�ȥ���D�A�ӵL�ݰ�� HTTP API �覡�C�o�b�Y�Ǳ��p�U�N�ܤ�K�A�Ҧp�Τ��A�ȶi����թΪ̤�ʭק�ƾڮw���e�C�ڭ̤]���˦b�豵Ĳ etcd �ɳq�L etcdctl �R�O�Ӽ��x�������ާ@�A�o�Ǿާ@�� HTTP API ��ڤW�O�������C

etcd ���ؤG�i��o��]���w�g�]�t�F etcdctl �u��A�S�����ܡA�i�H�q [github.com/coreos/etcd/releases](https://github.com/coreos/etcd/releases) �U���C

etcdctl ����p�U���R�O�A�j��W�����ƾڮw�ާ@�M�D�ƾڮw�ާ@�����A�᭱�N���O�i������C

```
$ etcdctl -h
NAME:
   etcdctl - A simple command line client for etcd.

USAGE:
   etcdctl [global options] command [command options] [arguments...]

VERSION:
   2.0.0-rc.1

COMMANDS:
   backup	backup an etcd directory
   mk		make a new key with a given value
   mkdir	make a new directory
   rm		remove a key
   rmdir	removes the key if it is an empty directory or a key-value pair
   get		retrieve the value of a key
   ls		retrieve a directory
   set		set the value of a key
   setdir	create a new or existing directory
   update	update an existing key with a given value
   updatedir	update an existing directory
   watch	watch a key for changes
   exec-watch	watch a key for changes and exec an executable
   member	member add, remove and list subcommands
   help, h	Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --debug			output cURL commands which can be used to reproduce the request
   --no-sync			don't synchronize cluster information before sending request
   --output, -o 'simple'	output response in the given format (`simple` or `json`)
   --peers, -C 			a comma-delimited list of machine addresses in the cluster (default: "127.0.0.1:4001")
   --cert-file 			identify HTTPS client using this SSL certificate file
   --key-file 			identify HTTPS client using this SSL key file
   --ca-file 			verify certificates of HTTPS-enabled servers using this CA bundle
   --help, -h			show help
   --version, -v		print the version
```

### �ƾڮw�ާ@
�ƾڮw�ާ@��¶����ȩM�ؿ��� CRUD �]�ŦX REST ���檺�@�M�ާ@�GCreate�^����ͩR�g�����޲z�C

etcd �b�䪺��´�W�ĥΤF�h���ƪ��Ŷ����c�]��������t�Τ��ؿ��������^�A�Τ���w����i�H����W���W�r�A�p `testkey`�A���ɹ�ڤW��b�ڥؿ� `/` �U���A�]�i�H�����w�ؿ����c�A�p `cluster1/node2/testkey`�A�h�N�Ыج������ؿ����c�C

*���GCRUD �Y Create, Read, Update, Delete�A�O�ŦX REST ���檺�@�M API �ާ@�C*

#### set
���w�Y���䪺�ȡC�Ҧp
```
$ etcdctl set /testdir/testkey "Hello world"
Hello world
```
������ﶵ�]�A�G
```
--ttl '0'			����Ȫ��W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
--swap-with-value value �Y����{�b���ȬO value�A�h�i��]�m�ާ@
--swap-with-index '0'	�Y����{�b�����ޭȬO���w���ޡA�h�i��]�m�ާ@
```

#### get
������w�䪺�ȡC�Ҧp
```
$ etcdctl set testkey hello
hello
$ etcdctl update testkey world
world
```

���䤣�s�b�ɡA�h�|�����C�Ҧp
```
$ etcdctl get testkey2
Error:  100: Key not found (/testkey2) [1]
```

������ﶵ��
```
--sort	�ﵲ�G�i��Ƨ�
--consistent �N�ШD�o���D�`�I�A�O��������e���@�P��
```

#### update
����s�b�ɡA��s�Ȥ��e�C�Ҧp
```
$ etcdctl set testkey hello
hello
$ etcdctl update testkey world
world
```

���䤣�s�b�ɡA�h�|�����C�Ҧp
```
$ etcdctl update testkey2 world
Error:  100: Key not found (/testkey2) [1]
```

������ﶵ��
```
--ttl '0'	�W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
```

#### rm
�R���Y����ȡC�Ҧp
```
$ etcdctl rm testkey

```

���䤣�s�b�ɡA�h�|�����C�Ҧp
```
$ etcdctl rm testkey2
Error:  100: Key not found (/testkey2) [8]
```

������ﶵ��
```
--dir		�p�G��O�Ӫťؿ��Ϊ���ȹ�h�R��
--recursive		�R���ؿ��M�Ҧ��l��
--with-value 	�ˬd�{�����ȬO�_�ǰt
--with-index '0'	�ˬd�{���� index �O�_�ǰt

```

#### mk
�p�G���w���䤣�s�b�A�h�Ыؤ@�ӷs����ȡC�Ҧp
```
$ etcdctl mk /testdir/testkey "Hello world"
Hello world
```
����s�b���ɭԡA����өR�O�|�����A�Ҧp
```
$ etcdctl set testkey "Hello world"
Hello world
$ ./etcdctl mk testkey "Hello world"
Error:  105: Key already exists (/testkey) [2]
```

������ﶵ��
```
--ttl '0'	�W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
```


#### mkdir
�p�G���w����ؿ����s�b�A�h�Ыؤ@�ӷs����ؿ��C�Ҧp
```
$ etcdctl mkdir testdir
```
����ؿ��s�b���ɭԡA����өR�O�|�����A�Ҧp
```
$ etcdctl mkdir testdir
$ etcdctl mkdir testdir
Error:  105: Key already exists (/testdir) [7]
```
������ﶵ��
```
--ttl '0'	�W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
```

#### setdir

�Ыؤ@����ؿ��A�L�צs�b�P�_�C

������ﶵ��
```
--ttl '0'	�W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
```

#### updatedir
��s�@�Ӥw�g�s�b���ؿ��C
������ﶵ��
```
--ttl '0'	�W�ɮɶ��]��쬰��^�A���t�m�]�q�{�� 0�^�h�ä��W��
```

#### rmdir
�R���@�Ӫťؿ��A�Ϊ���ȹ�C

�Y�ؿ����šA�|����
```
$ etcdctl set /dir/testkey hi
hi
$ etcdctl rmdir /dir
Error:  108: Directory not empty (/dir) [13]
```

#### ls
�C�X�ؿ��]�q�{���ڥؿ��^�U����Ϊ̤l�ؿ��A�q�{����ܤl�ؿ������e�C

�Ҧp
```
$ ./etcdctl set testkey 'hi'
hi
$ ./etcdctl set dir/test 'hello'
hello
$ ./etcdctl ls
/testkey
/dir
$ ./etcdctl ls dir
/dir/test
```

������ﶵ�]�A
```
--sort	�N��X���G�Ƨ�
--recursive	�p�G�ؿ��U���l�ؿ��A�h���k��X�䤤�����e
-p		����X���ؿ��A�b�̫�K�[ `/` �i��Ϥ�
```

### �D�ƾڮw�ާ@

#### backup
�ƥ� etcd ���ƾڡC

������ﶵ�]�A
```
--data-dir 		etcd ���ƾڥؿ�
--backup-dir 	�ƥ�����w���|
```
#### watch
�ʴ��@����Ȫ��ܤơA�@����ȵo�ͧ�s�A�N�|��X�̷s���Ȩðh�X�C

�Ҧp�A�Τ��s testkey ��Ȭ� Hello world�C
```
$ etcdctl watch testkey
Hello world
```

������ﶵ�]�A
```
--forever		�@���ʴ��A����Τ�� `CTRL+C` �h�X
--after-index '0'	�b���w index ���e�@���ʴ�
--recursive		��^�Ҧ�����ȩM�l���
```
#### exec-watch
�ʴ��@����Ȫ��ܤơA�@����ȵo�ͧ�s�A�N���浹�w�R�O�C

�Ҧp�A�Τ��s testkey ��ȡC
```
$etcdctl exec-watch testkey -- sh -c 'ls'
default.etcd
Documentation
etcd
etcdctl
etcd-migrate
README-etcdctl.md
README.md
```

������ﶵ�]�A
```
--after-index '0'	�b���w index ���e�@���ʴ�
--recursive		��^�Ҧ�����ȩM�l���
```

#### member
�q�L list�Badd�Bremove �R�O�C�X�B�K�[�B�R�� etcd ��Ҩ� etcd ���s���C

�Ҧp���a�Ұʤ@�� etcd �A�ȹ�ҫ�A�i�H�Φp�U�R�O�i��d�ݡC
```
$ etcdctl member list
ce2a822cea30bfca: name=default peerURLs=http://localhost:2380,http://localhost:7001 clientURLs=http://localhost:2379,http://localhost:4001

```
### �R�O�ﶵ
* `--debug`			��X cURL �R�O�A��ܰ���R�O���ɭԵo�_���ШD
* `--no-sync`			�o�X�ШD���e���P�B���s�H��
* `--output, -o 'simple'`	��X���e���榡 (`simple` ����l�H���A`json` ���i��json�榡�ѽX�A��Ū�ʦn�@��)
* `--peers, -C`			���w���s�����P��H���A�γr���j�} (�q�{��: "127.0.0.1:4001")
* `--cert-file` 			HTTPS �U�Ȥ�ݨϥΪ� SSL �ҮѤ��
* `--key-file`			HTTPS �U�Ȥ�ݨϥΪ� SSL �K�_���
* `--ca-file` 			�A�Ⱥݨϥ� HTTPS �ɡA�ϥ� CA ���i������
* `--help, -h`			������U�R�O�H��
* `--version, -v`		���L�����H��
