## �Q�μƾڨ��e���ӳƥ��B��_�B�E���ƾڨ�
�i�H�Q�μƾڨ���䤤���ƾڶi��i��ƥ��B��_�M�E���C

### �ƥ�
�����ϥ� `--volumes-from` �аO�ӳЫؤ@�ӥ[�� dbdata �e�������e���A�ñq�D��������e�ؿ���e���� /backup �ؿ��C�R�O�p�U�G
```
$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```
�e���Ұʫ�A�ϥΤF `tar` �R�O�ӱN dbdata ���ƥ����e���� /backup/backup.tar ���A�]�N�O�D����e�ؿ��U���W�� `backup.tar` �����C


### ��_
�p�G�n��_�ƾڨ�@�Ӯe���A�����Ыؤ@�ӱa���żƾڨ����e�� dbdata2�C
```
$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
```
�M��Ыإt�@�Ӯe���A���� dbdata2 �e���������ƾڨ��A�èϥ� `untar` �����ƥ����챾�����e�������C
```
$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
/backup/backup.tar
```
���F�d��/���ҫ�_���ƾڡA�i�H�A�Ұʤ@�Ӯe�������P�˪��e�����Ӭd��
```
$ sudo docker run --volumes-from dbdata2 busybox /bin/ls /dbdata
```