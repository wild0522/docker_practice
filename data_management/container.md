## �ƾڨ��e��
�p�G�A���@�ǫ����s���ƾڻݭn�b�e�������@�ɡA�̦n�Ыؼƾڨ��e���C

�ƾڨ��e���A���N�O�@�ӥ��`���e���A�M���ΨӴ��Ѽƾڨ��Ѩ䥦�e���������C

�����A�Ыؤ@�ӦW�� dbdata ���ƾڨ��e���G
```
$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
```
�M��A�b��L�e�����ϥ� `--volumes-from` �ӱ��� dbdata �e�������ƾڨ��C
```
$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
```
�i�H�ϥζW�L�@�Ӫ� `--volumes-from` �Ѽƨӫ��w�q�h�Ӯe���������P���ƾڨ��C
�]�i�H�q��L�w�g�����F�ƾڨ����e���ӯ��p�����ƾڨ��C
```
$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
```
*�`�N�G�ϥ� `--volumes-from` �ѼƩұ����ƾڨ����e���ۤv�ä��ݭn�O���b�B�檬�A�C

�p�G�R���F�������e���]�]�A dbdata�Bdb1 �M db2�^�A�ƾڨ��ä��|�Q�۰ʧR���C�p�G�n�R���@�Ӽƾڨ��A�����b�R���̫�@���ٱ����ۥ����e���ɨϥ� `docker rm -v` �R�O�ӫ��w�P�ɧR�����p���e���C
�o�i�H���Τ�b�e�������ɯũM���ʼƾڨ��C���骺�ާ@�N�b�U�@�`���i�����ѡC
