## �C�X���a�蹳
�ϥ� `docker images` ��ܥ��a�w�����蹳�C
```
$ sudo docker images
REPOSITORY       TAG      IMAGE ID      CREATED      VIRTUAL SIZE
ubuntu           12.04    74fe38d11401  4 weeks ago  209.6 MB
ubuntu           precise  74fe38d11401  4 weeks ago  209.6 MB
ubuntu           14.04    99ec81b80c55  4 weeks ago  266 MB
ubuntu           latest   99ec81b80c55  4 weeks ago  266 MB
ubuntu           trusty   99ec81b80c55  4 weeks ago  266 MB
...
```

�b�C�X�H�����A�i�H�ݨ�X�Ӧr�q�H��

* �Ӧ۩���ӭܮw�A��p ubuntu
* �蹳���аO�A��p 14.04
* ���� `ID` ���]�ߤ@�^
* �Ыخɶ�
* �蹳�j�p

�䤤�蹳�� `ID` �ߤ@���ѤF�蹳�A�`�N�� `ubuntu:14.04` �M `ubuntu:trusty` �㦳�ۦP���蹳 `ID`�A�������̹�ڤW�O�P�@�蹳�C

`TAG` �H���ΨӼаO�ӦۦP�@�ӭܮw�����P�蹳�C�Ҧp `ubuntu` �ܮw�����h���蹳�A�q�L `TAG` �H���ӰϤ��o�檩���A�Ҧp `10.04`�B`12.04`�B`12.10`�B`13.04`�B`14.04` ���C�Ҧp�U�����R�O���w�ϥ��蹳 `ubuntu:14.04` �ӱҰʤ@�Ӯe���C
```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
```

�p�G�����w���骺�аO�A�h�q�{�ϥ� `latest` �аO�H���C
