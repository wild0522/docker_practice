##�Ұʮe��
�Ұʮe������ؤ覡�A�@�جO����蹳�s�ؤ@�Ӯe���ñҰʡA�t�~�@�ӬO�N�b�פ�A�]stopped�^���e�����s�ҰʡC

�]�� Docker ���e����b�ӻ��q�ŤF�A�ܦh�ɭԥΤ᳣�O�H�ɧR���M�s�Ыخe���C

###�s�بñҰ�
�һݭn���R�O�D�n�� `docker run`�C

�Ҧp�A�U�����R�O��X�@�� �uHello World�v�A����פ�e���C
```
$ sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
Hello world
```
�o��b���a�������� `/bin/echo 'hello world'` �X�G�Pı���X����ϧO�C

�U�����R�O�h�Ұʤ@�� bash �׺ݡA���\�Τ�i��椬�C
```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
root@af8bae53bdd3:/#
```
�䤤�A`-t` �ﶵ��Docker���t�@�Ӱ��׺ݡ]pseudo-tty�^�øj�w��e�����зǿ�J�W�A `-i` �h���e�����зǿ�J�O�����}�C

�b�椬�Ҧ��U�A�Τ�i�H�q�L�ҳЫت��׺ݨӿ�J�R�O�A�Ҧp
```
root@af8bae53bdd3:/# pwd
/
root@af8bae53bdd3:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
```

��Q�� `docker run` �ӳЫخe���ɡADocker �b��x�B�檺�зǾާ@�]�A�G

* �ˬd���a�O�_�s�b���w���蹳�A���s�b�N�q�����ܮw�U��
* �Q���蹳�ЫبñҰʤ@�Ӯe��
* ���t�@�Ӥ��t�ΡA�æb�uŪ���蹳�h�~�������@�h�iŪ�g�h
* �q�J�D�D���t�m���������f�������@�ӵ������f��e�����h
* �q�a�}���t�m�@�� ip �a�}���e��
* ����Τ���w�����ε{��
* ���槹����e���Q�פ�

###�Ұʤw�פ�e��
�i�H�Q�� `docker start` �R�O�A�����N�@�Ӥw�g�פ�e���ҰʹB��C

�e�����֤߬��Ұ��檺���ε{�ǡA�һݭn���귽���O���ε{�ǹB��ҥ��ݪ��C�������~�A�èS���䥦���귽�C�i�H�b���׺ݤ��Q�� `ps` �� `top` �Ӭd�ݶi�{�H���C
```
root@ba267838cc1b:/# ps
  PID TTY          TIME CMD
    1 ?        00:00:00 bash
   11 ?        00:00:00 ps
```
�i���A�e�����ȹB��F���w�� bash ���ΡC�o�دS�I�ϱo Docker ��귽���Q�βv�����A�O�f�u���ꪺ���q�ŵ����ơC
