# �򥻬[�c

�����u�q�����س������}�u�q���[�c�]�p�C���p�`�N���� Kubernetes �b�[�c�譱���]�p�Ҽ{�C

## �򥻦Ҽ{
�p�G���ڭ̦ۤv�q�Y�]�p�@�M�e���޲z���x�A���p�U�X�Ӥ譱�O�ܮe���Q�쪺�G
* ���G���[�c�A�O���X�i�ʡF
* �޿趰����������� + ���z���G�����B�業���F
* �@�M�귽�իרt�ΡA�޲z���Ӯe���Ӥ��t����Ӹ`�I�W�F
* �@�M��e�����A�ȶi��⹳�M HA ���t�ΡC

## �B���z

�U���o�i�ϧ���i�ܤF Kubernetes ���B���z�C

![Kubernetes �[�c](../../_images/k8s_architecture.png)

�i���AKubernetes �����O�@�M���G���t�ΡA�Ѧh�Ӹ`�I�զ��A�`�I���������G�@���O�ݩ�޲z�������D�`�I/����`�I�]Master Node�^�F�@���O�ݩ�B�業�����u�@�`�I�]Worker Node�^�C

��M�A�������u�@�֩w���浹����`�I�h���F�A�u�@�`�I�t�d����í�w���ާ@���f�M��O�⹳�Y�i�C

�q�o�i�ϤW�A�ڭ̨S����o�{ Kubernetes ����󱱨�������G����{�A���O�ѩ�ƾګ�ݦۨ��N�O�@�M���G�����ƾڮw�]Etcd�^�A�]���i�H�ܮe���X�i����G����{�C

## �����
### �D�`�I�A��
�D�`�I�W�ݭn���Ѧp�U���޲z�A�ȡG
* apiserver �O��Өt�Ϊ���~���f�A���Ѥ@�M RESTful �� [Kubernetes API](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/api.md)�A�ѫȤ�ݩM�䥦�ե�եΡF
* scheduler �t�d��귽�i��իסA���t�Y�� pod ��Y�Ӹ`�I�W�C�O pluggable���A�N���۫ܮe����ܨ䥦��{�覡�F
* controller-manager �t�d�޲z����A�]�A endpoint-controller�]��s�A�ȩM pod �����p�H���^�M replication-controller�]���@�Y�� pod ���ƻs���t�m���ƭȡ^�C

### Etcd
�o�� Etcd �Y�@���ƾګ�ݡA�S�@������������C

�q�L Etcd �Ӧs�x�Ҧ����D�`�I�W�����A�H���A�ܮe����{�D�`�I�����G���X�i�C

�ե�i�H�۰ʪ��h���� Etcd �����ƭ��ܤƨ���o�q���A�åB��o��s�᪺�ƾڨӰ���������ާ@�C

## �u�@�`�I
* kubelet �O�u�@�`�I����ާ@�� agent�A�t�d���骺�e���ͩR�g���޲z�A�ھڱq�ƾڮw��������H���Ӻ޲z�e���A�äW�� pod �B�檬�A���F
* kube-proxy �O�@��²�檺�����X�ݥN�z�A�P�ɤ]�O�@�� Load Balancer�C���t�d�N�X�ݨ�Y�ӪA�Ȫ��ШD������t���u�@�`�I�W�� Pod�]�P�@�����ҡ^�C

![Proxy �N�z��A�Ȫ��ШD](../../_images/kube-proxy.png)
