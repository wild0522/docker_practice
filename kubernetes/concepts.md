# �򥻷���

![](../_images/kubernetes_design.jpg)

* �`�I�]Node�^�G�@�Ӹ`�I�O�@�ӹB�� Kubernetes �����D���C
* �e���ա]Pod�^�G�@�� Pod ������ѭY�z�e���զ����@�Ӯe���աA�P�Ӳդ����e���@�ɤ@�Ӧs�x��(volume)�C
* �e���եͩR�g���]pos-states�^�G�]�t�Ҧ��e�����A���X�A�]�A�e���ժ��A�����A�e���եͩR�g���A�ƥ�A���ҵ����A�H��replication controllers�C
* Replication Controllers�]replication-controllers�^�G�D�n�t�d���w�ƶq��pod�b�P�@�ɶ��@�_�B��C
* �A�ȡ]services�^�G�@��Kubernetes�A�ȬO�e�����޿誺���ũ⹳�A�P�ɤ]��~���ѳX�ݮe���ժ������C
* ���]volumes�^�G�@�Ө��N�O�@�ӥؿ��A�e����䦳�X���v���C
* ���ҡ]labels�^�G���ҬO�Ψӳs���@�չ�H���A��p�e���աC���ҥi�H�Q�ΨӲ�´�M��ܤl��H�C
* ���f�v���]accessing_the_api�^�G�ݤf�Aip�a�}�M�N�z��������W�h�C
* web �ɭ��]ux�^�G�Τ�i�H�q�L web �ɭ��ާ@Kubernetes�C
* �R�O��ާ@�]cli�^�G`kubecfg`�R�O�C


## �`�I
�b Kubernetes ���A�`�I�O��ڤu�@���I�A�H�e�s�� Minion�C�`�I�i�H�O�������Ϊ̪��z�����A�̿��@�Ӷ��s���ҡC�C�Ӹ`�I�����@�ǥ��n���A�ȥH�B��e���աA�åB���̳��i�H�q�L�D�`�I�Ӻ޲z�C���n�A�ȥ]�A Docker�Akubelet �M�N�z�A�ȡC

### �e�����A

�e�����A�ΨӴy�z�`�I����e���A�C�{�b�A�䤤�]�t�T�ӫH���G

#### �D��IP

�D��IP�ݭn�����x�Ӭd�ߡAKubernetes�⥦�@�����A���@�����ӫO�s�C�p�GKubernetes�S���B��b�����x�W�A�`�IID�N�O���ݪ��CIP�a�}�i�H�ܤơA�åB�i�H�]�t�h��������IP�a�}�A�p���@IP�A�p��IP�A�ʺAIP�Aipv6�����C

#### �`�I�g��

�q�`�ӻ��`�I�� `Pending`�A`Running`�A`Terminated`�T�Ӷg���A�p�GKubernetes�o�{�F�@�Ӹ`�I�åB��i�ΡA����Kubernetes�N�⥦�аO�� `Pending`�C�M��b�Y�Ӯɨ�AKubernetes�N�|�аO�䬰 `Running`�C�`�I�������g���٬� `Terminated`�C�@�Ӥw�gterminated���`�I���|�����M�իץ���ШD�A�åB�w�g�b��W�B�檺�e���դ]�|�R���C

#### �`�I���A

�`�I�����A�D�n�O�ΨӴy�z�B�� `Running`���`�I�C��e�i�Ϊ��� `NodeReachable` �M `NodeReady` �C�H��i��|�W�[��L���A�C`NodeReachable` ��ܶ��s�i�F�C`NodeReady`���kubelet��^ StatusOk�åBHTTP���A�ˬd���d�C

### �`�I�޲z

�`�I�ëDKubernetes�ЫءA�ӬO�Ѷ����x�ЫءA�Ϊ̴N�O���z�����B�������C�bKubernetes���A�`�I�ȶȬO�@���O���A�`�I�Ыؤ���AKubernetes�|�ˬd��O�_�i�ΡC�bKubernetes���A�`�I�Φp�U���c�O�s�G

```
{
  "id": "10.1.2.3",
  "kind": "Minion",
  "apiVersion": "v1beta1",
  "resources": {
    "capacity": {
      "cpu": 1000,
      "memory": 1073741824
    },
  },
  "labels": {
    "name": "my-first-k8s-node",
  },
}
```

Kubernetes����`�I�i�Ψ̿��id�C�b��e���������A����ӱ��f�i�H�ΨӺ޲z�`�I�G�`�I����MKube�޲z�C

### �`�I����

�bKubernetes�D�`�I���A�`�I����O�ΨӺ޲z�`�I���ե�C�D�n�]�t�G
* ���s�d�򤺸`�I�P�B
* ��`�I�ͩR�g���޲z

�`�I����@�ӦP�B���M�A�D�n��ť�Ҧ������x��������ҡA�|�ھڸ`�I���A�ЫةM�R���C�i�H�q�L `--node_sync_period`�лx�ӱ���ӽ��M�C�p�G�@�ӹ�Ҥw�g�ЫءA�`�I����N�|����Ыؤ@�ӵ��c�C�P�˪��A�p�G�@�Ӹ`�I�Q�R���A�`�I����]�|�R���ӵ��c�C�bKubernetes�Ұʮɥi�γq�L `--machines`�аO����ܫ��w�`�I�C�P�˥i�H�ϥ� `kubectl`�Ӥ@���@�����K�[�`�I�A��̬O�ۦP���C�q�L�]�m `--sync_nodes=false`�аO�ӸT��s�������`�I�P�B�A�A�]�i�H�ϥ�api/kubectl �R�O��ӼW�R�`�I�C

## �e����

�bKubernetes���A�ϥΪ��̤p���O�e���աA�e���լO�ЫءA�իסA�޲z���̤p���C
�@�Ӯe���ըϥάۦP��Dokcer�e���æ@�ɨ��]�����I�^�C�@�Ӯe���լO�@�ӯS�w�B�Ϊ����]���X�A�]�t�@�өΦh�Ӯe���C

�M�B�檺�e�������A�@�Ӯe���ճQ�{���u���ܵu���B��g���C�e���ճQ�իר�@�ո`�I�B��A���D�e�����ͩR�g�������Ϊ̨�Q�R���C�p�G�`�I�����A�B��b��W���e���ձN�|�Q�R���Ӥ��O���s�իסC�]�]�\�b�N�Ӫ��������|�K�[�e���ժ����ʡ^�C

### �e���ճ]�p����J

### �귽�@�ɩM�q�H

�e���եD�n�O���F�ƾڦ@�ɩM���̤������q�H�C

�b�@�Ӯe���դ��A�e�����ϥάۦP�������a�}�M�ݤf�A�i�H�q�L���a�����Ӭۤ��q�H�C�C�Ӯe���ճ����W�ߪ�ip�A�i�γq�L�����өM��L���z�D���Ϊ̮e���q�H�C

�e���զ��@�զs�x���]�����I�^�A�D�n�O���F���e���b���Ҥ���i�H���ᥢ�ƾڡC

### �e���պ޲z

�e���լO�@�ӹB�κ޲z�M���p�����h���⹳�A�P�ɤ]�O�@�ծe�������f�C�e���լO���p�B�������Y���̤p���C

### �e���ժ��ϥ�

�e���եi�H�q�L�զX�Ӻc�ؽ������B�ΡA�䥻�Ӫ��N�q�]�t�G

* ���e�޲z�A���M�ƾڥ[���H�Υ��a�w�s�޲z���C
* ��x�M�ˬd�I�ƥ��A���Y�A�ַӵ��C
* ��ť�ƾ��ܤơA���ܤ�x�A��x�M�ʱ��N�z�A�����o�G���C
* �N�z�A����
* ����A�޲z�A�t�m�H�Χ�s

### ���N���

�����򤣦b�@�ӳ�@���e���̹B��h�ӵ{�ǡH

* 1.�z���ơC���F�Ϯe���դ����e���O���@�P����¦�]�I�M�A�ȡA��p�i�{�޲z�M�귽�ʱ��C�o�˳]�p�O���F�Τ᪺�K�Q�ʡC
* 2.�Ѱ��n�󤧶����̿�C�C�Ӯe�����i�୫�s�c�ةM�o�G�AKubernetes����������o�G�M����s�]�N�ӡ^�C
* 3.��K�ϥΡC�Τᤣ���B��W�ߪ��{�Ǻ޲z�A�]���ξ�ߨC�ӹB�ε{�Ǫ��h�X���A�C
* 4.���ġC�Ҽ{���¦�]�I����h��¾�d�A�e�������n���q�ơC

### �e���ժ��ͩR���A

�]�A�Y�z���A�ȡGpending�Brunning�Bsucceeded�Bfailed�C

#### pending

�e���դw�g�Q�`�I�����A�����@�өΦh�Ӯe���٨S���B��_�ӡC�o�N�]�t�Y�Ǹ`�I���b�U���蹳���ɶ��A�o�ر��η|�̿��������p�C

#### running

�e���դw�g�Q�իר�`�I�A�åB�Ҧ����e�����w�g�ҰʡC�ܤ֦��@�Ӯe���B��B�檬�A�]�Ϊ̳B�󭫱Ҫ��A�^�C

#### succeeded

�Ҧ����e�������`�h�X�C

#### failed

�e���դ��Ҧ��e�����N�~���_�F�C

### �e���եͩR�g��

�q�`�ӻ��A�p�G�e���ճQ�ЫؤF�N���|�۰ʾP���A���D�Q�Y�ئ欰�X�o�A��Ĳ�o���ر��p�i��O�H���A�Ϊ̽ƻs����Ҭ��C�ߤ@�ҥ~���O�e���ե� succeeded���A���\�h�X�A�Ϊ̦b�@�w�ɶ������զh���̵M���ѡC

�p�G�Y�Ӹ`�I�����Ϊ̤���s���A����`�I����N�|�аO��W���e���ժ����A�� `failed`�C

�|�Ҧp�U�C

* �e���ժ��A `running`�A�� 1 �e���A�e�����`�h�X
     * �O�������ƥ�
     * �p�G���ҵ������G
        * �l�סG���Үe���A�e���իO�� `running`
        * ���ѮɡG�e�����ܬ� `succeeded`
        * �q���G�e�����ܬ� `succeeded`
* �e���ժ��A `running`�A��1�e���A�e�����`�h�X
     * �O�����Ѩƥ�
     * �p�G���ҵ������G
        * �l�סG���Үe���A�e���իO�� `running`
        * ���ѮɡG���Үe���A�e���իO�� `running`
        * �q���G�e�����ܬ� `failed`
* �e���ժ��A `running`�A��2�e���A��1�e�����`�h�X
     * �O�����Ѩƥ�
     * �p�G���ҵ������G
        * �l�סG���Үe���A�e���իO�� `running`
        * ���ѮɡG���Үe���A�e���իO�� `running`
        * �q���G�e���իO�� `running`
    * ��2�e���h�X
        * �O�����Ѩƥ�
        * �p�G���ҵ������G
            * �l�סG���Үe���A�e���իO�� `running`
            * ���ѮɡG���Үe���A�e���իO�� `running`
            * �q���G�e�����ܬ� `failed`
* �e���ժ��A `running`�A�e�����s����
    * �аO�e�����~���_
    * �O�����s�����ƥ�
    * �p�G���ҵ������G
        * �l�סG���Үe���A�e���իO�� `running`
        * ���ѮɡG���Үe���A�e���իO�� `running`
        * �q���G�O�����~�ƥ�A�e�����ܬ� `failed`
* �e���ժ��A `running`�A�@���ϽL����
    * �����Ҧ��e��
    * �O���ƥ�
    * �e�����ܬ� `failed`
    * �p�G�e���չB��b�@�ӱ���U�A�e���ձN�|�b��L�a�譫�s�Ы�
* �e���ժ��A `running`�A�������`�I�q���X
    * �`�I�������W��
    * �`�I����аO�e���� `failed`
    * �p�G�e���չB��b�@�ӱ���U�A�e���ձN�|�b��L�a�譫�s�Ы�

## Replication Controllers
## �A��
## ��
## ����
## ���f�v��
## web�ɭ�
## �R�O��ާ@
