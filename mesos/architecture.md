## Mesos �򥻭�z�P�[�c

�����AMesos �ۨ��u�O�@�Ӹ귽�ի׮ج[�A�ëD�@��M���㪺���κ޲z���x�A�����O����F�����C���O���i�H����e������U�����κ޲z�Ϊ̤����󥭥x��X�A�@�_�u�@�A�����귽�ϥήĲv�C

### �[�c
![mesos-arch](../_images/mesos-architecture.png)
master-slave �[�c�Amaster �ϥ� zookeeper �Ӱ� HA�C

master ��W�B��b�޲z�`�I�W�Aslave �B��b�U�ӭp����ȸ`�I�W�C

�U�ب�����Ȫ��޲z���x�A�Y framework �� master �椬�A�ӥӽи귽�C


### �򥻳椸

#### master
�t�d���骺�귽�իשM�޿豱��C

#### slave
�t�d�׳����`�I�W���귽�� master�A�ít�d�j���귽�Ӱ�����骺���ȡC

�j�������M�N�O�U�خe������F�C

#### framework
framework �O��ڷF�����A�]�A��ӥD�n�ե�G

* scheduler�G���U��D�`�I�A���ݤ��t�귽�F
* executor�G�b slave �`�I�W���楻framework �����ȡC

framework ����ءG�@�جO��귽�ݨD�i�H scale up �Ϊ� down ���]Hadoop�BSpark�^�F�@�جO��귽�ݨD�j�p�O�T�w���]MPI�^�C

### �ի�
���@�Ӹ귽�ի׮ج[�ӻ��A�̮֤ߪ��N�O�ի׾���A����ֳt���Ī�������Y�� framework �귽�����t�]�̦n�O��q�쥦����ڻݨD�^�C

#### ��h�ի׺�k�G
master ���իפ@�j���귽���Y�� framework�Aframework �ۤv�A��{�������Ӳɫ׽իסC

�ի׾���������C�q�{�O DRF�C

#### �򥻽ի׹L�{
�ի׳q�L offer �覡�椬�G

* master ���Ѥ@�� offer�]�@�ո귽�^ �� framework�F
* framework �i�H�M�w�n���n�A�p�G�������ܡA��^�@�Ӵy�z�A�����ۤv�Ʊ�p��ϥΩM���t�o�Ǹ귽�]�i�H�����u�Ʊ�ϥγ����귽�A�h�h�X�Ӫ��|�Q master ���^�^�F
* master �h�ھ� framework �����t���p�o�e�� slave�A�H�ϥ� framework �� executor �ӫ��Ӥ��t���귽����������ȡC

#### �L�o��
framework �i�H�q�L�L�o������i�D master �����귽���n�A��p�Ʊ���t�L�Ӫ� offer �����Ӹ귽�A�Ϊ̦ܤ֦��h�ָ귽�C

�D�n�O���F�[�t�귽���t���椬�L�{�C

#### �^�Y����
master �i�H�q�L�^���p��`�I�W�����ȨӰʺA�վ�������ȩM�u�����Ȫ����G�C


### HA

#### master
master �`�I�s�b���I���İ��D�A�ҥH�֩w�n�W HA�A�ثe�D�n�O�ϥ� zookpeer �Ӽ��ƥ��C

�P�� master �`�I�i�H�q�L slave �M framework �o�Ӫ��������ؤ������A�]����঳�h�֩O�H�o�̤��ϥμƾڮw�i��O�קK�ޤJ�����סC�^�C

#### framework �q��
framework �����������ġAmaster �N�o������ scheduler �ӳq���C
