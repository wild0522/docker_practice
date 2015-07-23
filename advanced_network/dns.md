## �t�m DNS
Docker �S�����C�Ӯe���M���w���蹳�A������۩w�q�t�m�e�����D���W�M DNS �t�m�O�H
���Z�N�O���Q�ε������ӱ�����Ӯe���� 3 �Ӭ����t�m���C

�b�e�����ϥ� mount �R�O�i�H�ݨ챾���H���G
```
$ mount
...
/dev/disk/by-uuid/1fec...ebdf on /etc/hostname type ext4 ...
/dev/disk/by-uuid/1fec...ebdf on /etc/hosts type ext4 ...
tmpfs on /etc/resolv.conf type tmpfs ...
...
```
�o�ؾ���i�H���J�D�D�� DNS �H���o�ͧ�s��A�Ҧ� Docker �e���� dns �t�m�q�L `/etc/resolv.conf` ���ߨ�o���s�C

�p�G�Τ�Q�n��ʫ��w�e�����t�m�A�i�H�Q�ΤU�����ﶵ�C

`-h HOSTNAME or --hostname=HOSTNAME`
�]�w�e�����D���W�A���|�Q�g��e������ `/etc/hostname` �M `/etc/hosts`�C�����b�e���~���ݤ���A�J���|�b `docker ps` ����ܡA�]���|�b��L���e���� `/etc/hosts` �ݨ�C

`--link=CONTAINER_NAME:ALIAS`
�ﶵ�|�b�Ыخe�����ɭԡA�K�[�@�Ө�L�e�����D���W�� `/etc/hosts` ��󤤡A���s�e�����i�{�i�H�ϥΥD���W ALIAS �N�i�H�s�����C

`--dns=IP_ADDRESS`
�K�[ DNS �A�Ⱦ���e���� `/etc/resolv.conf` ���A���e���γo�ӪA�Ⱦ��ӸѪR�Ҧ����b `/etc/hosts` �����D���W�C

`--dns-search=DOMAIN`
�]�w�e�����j����A��]�w�j���쬰 `.example.com` �ɡA�b�j���@�ӦW�� host ���D���ɡADNS ���ȷj��host�A�ٷ|�j�� `host.example.com`�C
�`�N�G�p�G�S���W�z�̫� 2 �ӿﶵ�ADocker �|�q�{�ΥD���W�� `/etc/resolv.conf` �Ӱt�m�e���C
