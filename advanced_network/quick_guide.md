## �ֳt�t�m���n

�U���O�@�Ӹ� Docker �����������R�O�C��C

�䤤���ǩR�O�ﶵ�u���b Docker �A�ȱҰʪ��ɭԤ~��t�m�A�ӥB���న�W�ͮġC
* `-b BRIDGE or --bridge=BRIDGE` --���w�e������������
* `--bip=CIDR` --�w�� docker0 �����X
* `-H SOCKET... or --host=SOCKET...` --Docker �A�Ⱥݱ����R�O���q�D
* `--icc=true|false` --�O�_����e�������i��q�H
* `--ip-forward=true|false` --�ЬݤU��e���������q�H
* `--iptables=true|false` --�T�� Docker �K�[ iptables �W�h
* `--mtu=BYTES` --�e���������� MTU

�U��2�өR�O�ﶵ�J�i�H�b�ҰʪA�Ȯɫ��w�A�]�i�H Docker �e���Ұʡ]`docker run`�^�ɭԫ��w�C�b Docker �A�ȱҰʪ��ɭԫ��w�h�|�����q�{�ȡA�᭱���� `docker run` �ɥi�H�л\�]�m���q�{�ȡC
* `--dns=IP_ADDRESS...` --�ϥΫ��w��DNS�A�Ⱦ�
* `--dns-search=DOMAIN...` --���wDNS�j����

�̫�o�ǿﶵ�u���b `docker run` ����ɨϥΡA�]�����O�w��e�����S�ʤ��e�C
* `-h HOSTNAME or --hostname=HOSTNAME` --�t�m�e���D���W
* `--link=CONTAINER_NAME:ALIAS` --�K�[��t�@�Ӯe�����s��
* `--net=bridge|none|container:NAME_or_ID|host` --�t�m�e���������Ҧ�
* `-p SPEC or --publish=SPEC` --�M�g�e���ݤf��J�D�D��
* `-P or --publish-all=true|false` --�M�g�e���Ҧ��ݤf��J�D�D��
