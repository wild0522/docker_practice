## �������a�蹳
�p�G�n�������a���蹳�A�i�H�ϥ� `docker rmi` �R�O�C�`�N `docker rm` �R�O�O�����e���C
```
$ sudo docker rmi training/sinatra
Untagged: training/sinatra:latest
Deleted: 5bc342fa0b91cabf65246837015197eecfa24b2213ed6a51a8974ae250fedd8d
Deleted: ed0fffdcdae5eb2c3a55549857a8be7fc8bc4241fb19ad714364cbfd7a56b22f
Deleted: 5c58979d73ae448df5af1d8142436d81116187a7633082650549c52c3a2418f0
```

*�`�N�G�b�R���蹳���e�n���� `docker rm` �R���̿��o���蹳���Ҧ��e���C

##�M�z�Ҧ������L���Ҫ����a�蹳

`docker images` �i�H�C�X���a�Ҧ����蹳�A�䤤�ܥi��|�]�t���ܦh�������A�������L���Ҫ��蹳�A�j�q���ڵۺϽL�Ŷ��C

�ϥΤU�����R�O�i�H�M�z�Ҧ������L���Ҫ����a�蹳

```
$ sudo docker rmi $(docker images -q -f "dangling=true")
```

�䤤 `-q` �M `-f` �O�Y�g, ���㪺�R�O���i�H�g�ۤU���o�ˡA�O���O��e���z�Ѥ@�I�H

```
$ sudo docker rmi $(docker images --quiet --filter "dangling=true")
```