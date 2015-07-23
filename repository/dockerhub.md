## Docker Hub
�ثe Docker �x����@�F�@�Ӥ��@�ܮw [Docker Hub](https://hub.docker.com/)�A�䤤�w�g�]�A�F�W�L 15,000 ���蹳�C�j�����ݨD�A���i�H�q�L�b Docker Hub �������U���蹳�ӹ�{�C

### �n��
�i�H�q�L���� `docker login` �R�O�ӿ�J�Τ�W�B�K�X�M�l�c�ӧ������U�M�n���C
���U���\��A���a�Τ�ؿ��� `.dockercfg` ���N�O�s�Τ᪺�{�ҫH���C

### �򥻾ާ@
�Τ�L�ݵn���Y�i�q�L `docker search` �R�O�Ӭd��x��ܮw�����蹳�A�çQ�� `docker pull` �R�O�ӱN���U���쥻�a�C

�Ҧp�H centos ��������i��j���G
```
$ sudo docker search centos
NAME                                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                                          The official build of CentOS.                   465       [OK]
tianon/centos                                   CentOS 5 and 6, created using rinse instea...   28
blalor/centos                                   Bare-bones base CentOS 6.5 image                6                    [OK]
saltstack/centos-6-minimal                                                                      6                    [OK]
tutum/centos-6.4                                DEPRECATED. Use tutum/centos:6.4 instead. ...   5                    [OK]
...
```
�i�H�ݨ��^�F�ܦh�]�t����r���蹳�A�䤤�]�A�蹳�W�r�B�y�z�B�P�š]��ܸ��蹳�����w��{�ס^�B�O�_�x��ЫءB�O�_�۰ʳЫءC
�x�誺�蹳�����O�x�趵�زճЫةM���@���Aautomated �귽���\�Τ������蹳���ӷ��M���e�C

�ھڬO�_�O�x�责�ѡA�i�N�蹳�귽���������C
�@�جO���� centos �o�˪���¦�蹳�A�Q�٬���¦�ή��蹳�C�o�ǰ�¦�蹳�O�� Docker ���q�ЫءB���ҡB����B���ѡC�o�˪��蹳�����ϥγ�ӳ���@���W�r�C
�٦��@�������A��p `tianon/centos` �蹳�A���O�� Docker ���Τ�Ыبú��@���A�����a���Τ�W�٫e��C�i�H�q�L�e�� `user_name/` �ӫ��w�ϥάY�ӥΤᴣ�Ѫ��蹳�A��p tianon �Τ�C

�t�~�A�b�d�䪺�ɭԳq�L `-s N` �Ѽƥi�H���w����ܵ����� `N` �P�H�W���蹳�C

�U���x�� centos �蹳�쥻�a�C
```
$ sudo docker pull centos
Pulling repository centos
0b443ba03958: Download complete
539c0211cd76: Download complete
511136ea3c5a: Download complete
7064731afe90: Download complete
```
�Τ�]�i�H�b�n����q�L `docker push` �R�O�ӱN�蹳���e�� Docker Hub�C

### �۰ʳЫ�
�۰ʳЫء]Automated Builds�^�\����ݭn�g�`�ɯ��蹳���{�Ǩӻ��A�Q����K�C
���ɭԡA�Τ�ЫؤF�蹳�A�w�ˤF�Y�ӳn��A�p�G�n��o�G�s�����h�ݭn��ʧ�s�蹳�C�C

�Ӧ۰ʳЫؤ��\�Τ�q�L Docker Hub ���w���ܤ@�ӥؼк����]�ثe��� [GitHub](github.org) �� [BitBucket](bitbucket.org)�^�W�����ءA�@�����صo�ͷs������A�h�۰ʰ���ЫءC

�n�t�m�۰ʳЫءA�]�A�p�U���B�J�G
* �Ыبõn�� Docker Hub�A�H�Υؼк����F
* �b�ؼк������s���b��� Docker Hub�F
* �b Docker Hub �� [�t�m�@�Ӧ۰ʳЫ�](https://registry.hub.docker.com/builds/add/)�F
* ����@�ӥؼк����������ء]�ݭn�t Dockerfile�^�M����F
* ���w Dockerfile ����m�A�ô���ЫءC

����A�i�H �bDocker Hub �� [�۰ʳЫح���](https://registry.hub.docker.com/builds/) �����ܨC���Ыت����A�C
