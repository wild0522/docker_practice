## �ϥ�
�b�ϥ� swarm �޲z���s�e�A�ݭn�ⶰ�s���Ҧ����`�I�� docker daemon ����ť�覡��אּ `0.0.0.0:2375`�C

�i�H����ؤ覡�F��o�ӥت��A�Ĥ@�جO�b�Ұ�docker daemon���ɭԫ��w
```sh
sudo docker -H 0.0.0.0:2375&
```

�ĤG�ؤ覡�O�����ק� Docker ���t�m���(Ubuntu �W�O `/etc/default/docker`�A��L������ Linux �W�������P)

�b��󪺳̫�K�[�U���o�y�N�X�G
```sh
DOCKER_OPTS="-H 0.0.0.0:2375 -H unix:///var/run/docker.sock"
```


�ݭn�`�N���O�A�@�w�n�b�Ҧ��Ʊ�Q Swarm �޲z���`�I�W�i�檺�C�ק蠟��n���� Docker
```sh
sudo service docker restart
```

Docker ���s�޲z�ݭn�ϥΪA�ȵo�{(Discovery service backend)�\��ASwarm����H�U���X�ؤ覡�GDockerHub ���Ѫ��A�ȵo�{�\��A���a�����Aetcd�Acounsel�Azookeeper �M IP �C��A����|�Բ����ѫe��ؤ覡�A��L���Ϊk���O�j�P�p�����C

�����@�U�������窺���ҡA��������]�A�T�x�����AIP�a�}���O��192.168.1.84,192.168.1.83�M192.168.1.124.�Q�γo�T�x�����զ��@��docker���s�A�䤤83�o�x�����P�ɥR��swarm manager�`�I�C

### �ϥ� DockerHub ���Ѫ��A�ȵo�{�\��

#### �Ыض��s token

�b�W���T�x������������@�x�����W������ `swarm create` �R�O������@�Ӷ��s�лx�C�o���R�O���槹����ASwarm �|�e�� DockerHub �W���m���o�{�A�Ȥ�����@�ӥ��y�ߤ@�� token�A�ΨӼ��ѭn�޲z�����s�C
```sh
sudo docker run --rm swarm create
```

�ڭ̦b84�o�x�����W����o���R�O�A��X�p�U�G
```sh
rio@084:~$ sudo docker run --rm swarm create
b7625e5a7a2dc7f8c4faacf2b510078e
```

�i�H�ݨ�ڭ̪�^�� token �O `b7625e5a7a2dc7f8c4faacf2b510078e`�A�C����^�����G���O���@�˪��C�o�� token �@�w�n�O��A�᭱���ާ@���|�Ψ�o�� token�C

#### �[�J���s

�b�Ҧ��n�[�J���s���`�I�W������ `swarm join` �R�O�A��ܭn��o�x�����[�J�o�Ӷ��s���C�b�������礤�A�N�O�n�b 83�B84 �M 124 �o�T�x�����W����U�����o���R�O�G
```sh
sudo docker run --rm swarm join addr=ip_address:2375 token://token_id
```
�䤤�� ip_address ��������o���R�O�������� IP�Atoken_id �����W�@�B���� `swarm create` ��^�� token�C

�b83�o�x�����W�������浲�G�p�U�G
```sh
rio@083:~$ sudo docker run --rm swarm join --addr=192.168.1.83:2375 token://b7625e5a7a2dc7f8c4faacf2b510078e
time="2015-05-19T11:48:03Z" level=info msg="Registering on the discovery service  every 25 seconds..." addr="192.168.1.83:2375" discovery="token://b7625e5a7a2dc7 f8c4faacf2b510078e"
```
�o���R�O���|�۰ʪ�^�A�n�ڭ̦ۤv���� `Ctrl+C` ��^�C

#### �Ұ�swarm manager
�]���ڭ̭n�ϥ� 83 �o�x�����R�� swarm �޲z�`�I�A�ҥH�ݭn�b83�o�x�����W������ `swarm manage` �R�O�G
```sh
sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
```
���浲�G�p�U�G
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 swarm manage token://b7625e5a7a2dc7f8c4faacf2b510078e
83de3e9149b7a0ef49916d1dbe073e44e8c31c2fcbe98d962a4f85380ef25f76
```
�o���R�O�p�G���榨�\�|��^�w�g�Ұʪ� Swarm ���e���� ID�A���ɾ�Ӷ��s�w�g�Ұʰ_�ӤF�C

�{�b�q�L `docker ps` �R�O�ӬݤU���S���Ұʦ��\�C
```sh
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
83de3e9149b7        swarm:latest        "/swarm manage token   4 minutes ago       Up 4 minutes        0.0.0.0:2376->2375/tcp   stupefied_stallman
```
�i�H�ݨ�ASwarm �w�g���\�ҰʡC
�b���� `Swarm manage` �o���R�O���ɭԡA���X�I�ݭn�`�N���G

* �o���R�O�ݭn�b�R�� swarm �޲z�̪������W����
* Swarm �n�H daemon ���Φ�����
* �M�g���ݤf�i�H�ϥ��N�����F 2375 �H�~���åB�O���Q���Ϊ��ݤf�A���@�w����O 2375 �o�Ӻݤf�A�]�� 2375 �w�g�Q Docker ���������ΤF�C

���s�Ұʦ��\�H��A�{�b�ڭ̥i�H�b����@�x�`�I�W�ϥ� `swarm list` �R�O�d�ݶ��s�����`�I�F�A������b 124 �o�x�����W���� `swarm list` �R�O�G
```sh
rio@124:~$ sudo docker run --rm swarm list token://b7625e5a7a2dc7f8c4faacf2b510078e
192.168.1.84:2375
192.168.1.124:2375
192.168.1.83:2375
```
��X���G�C�X��IP�a�}���O�ڭ̨ϥ� `swarm join` �R�O�[�J���s��������IP�a�}�C

�{�b�ڭ̥i�H�b����@�x�w�ˤF Docker �������W���q�L�R�O(�R�O���n����swarm manager������IP�a�})�Ӧb���s���B��container�F�C
��������A�ڭ̦b 192.168.1.85 �o�x�����W�ϥ� `docker info` �R�O�Ӭd�ݶ��s�����`�I���H���C

�䤤 info �]�i�H������L�� Docker ������R�O�C
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 info
Containers: 8
Strategy: spread
Filters: affinity, health, constraint, port, dependency
Nodes: 2
 sclu083: 192.168.1.83:2375
  �| Containers: 1
  �| Reserved CPUs: 0 / 2
  �| Reserved Memory: 0 B / 4.054 GiB
 sclu084: 192.168.1.84:2375
  �| Containers: 7
  �| Reserved CPUs: 0 / 2
  �| Reserved Memory: 0 B / 4.053 GiB
```
���G��X��ܳo�Ӷ��s���u����Ӹ`�I�AIP�a�}���O�O 192.168.1.83 �M 192.168.1.84�A���G����r�A�ڭ̩�����T�x�����[�J�F�o�Ӷ��s�A�٦� 124 �o�@�x�����O�H
�g�L�Ƭd�A�o�{�O�ѤF�ק� 124 �o�x�����W���� docker daemon ����ť�覡�A�u�n���ӤW�����B�J�ק�g docker daemon ����ť�覡�N�i�H�F�C

�b�ϥγo�Ӥ�k���ɭԡA�ϥ�swarm create�i��|�]����������]�|�X�{������U�����o�Ӱ��D�G
```sh
rio@227:~$ sudo docker run --rm swarm create
[sudo] password for rio:
time="2015-05-19T12:59:26Z" level=fatal msg="Post https://discovery-stage.hub.docker.com/v1/clusters: dial tcp: i/o timeout"
```

### �ϥΤ��

�ĤG�ؤ�k�۹��Ĥ@�ؤ�k�n²��o�h�A�]���|�X�{������W�������D�C

�Ĥ@�B�G�b swarm �޲z�`�I�W�s�ؤ@�Ӥ��A��n�[�J���s������ IP �a�}�M�ݤf���g�J��󤤡A��������N�O�n�b83�o�x�����W���ާ@�G
```sh
rio@083:~$ echo 192.168.1.83:2375 >> cluster
rio@083:~$ echo 192.168.1.84:2375 >> cluster
rio@083:~$ echo 192.168.1.124:2375 >> cluster
rio@083:~$ cat cluster
192.168.1.83:2375
192.168.1.84:2375
192.168.1.124:2375
```

�ĤG�B�G�b083�o�x�����W������ `swarm manage` �o���R�O�G
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage file:///tmp/cluster
364af1f25b776f99927b8ae26ca8db5a6fe8ab8cc1e4629a5a68b48951f598ad
```
�ϥ�`docker ps`�Ӭd�ݦ��S���Ұʦ��\�G
```sh
rio@083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS              PORTS                    NAMES
364af1f25b77        swarm:latest        "/swarm manage file:   About a minute ago   Up About a minute   0.0.0.0:2376->2375/tcp   happy_euclid
```
�i�H�ݨ�A���ɾ�Ӷ��s�w�g�Ұʦ��\�C

�b�ϥγo���R�O���ɭԻݭn�`�N���O�`�N�G�o�̤@�w�n�ϥ�-v�R�O�A�]��cluster���O�b�����W���A�Ұʪ��e���q�{�O�X�ݤ��쪺�A�ҥH�n�q�L-v�R�O�@�ɡC

���U�Ӫ��N�i�H�b����@�x�w�ˤFdocker�������W���q�L�R�O�ϥζ��s�A�P�˪��A�b85�o�x�����W����docker info�R�O�d�ݶ��s���`�I�H���G
```sh
rio@s085:~$ sudo docker -H 192.168.1.83:2376 info
Containers: 9
Strategy: spread
Filters: affinity, health, constraint, port, dependency
Nodes: 3
 atsgxxx: 192.168.1.227:2375
  �| Containers: 0
  �| Reserved CPUs: 0 / 4
  �| Reserved Memory: 0 B / 2.052 GiB
 sclu083: 192.168.1.83:2375
  �| Containers: 2
  �| Reserved CPUs: 0 / 2
  �| Reserved Memory: 0 B / 4.054 GiB
 sclu084: 192.168.1.84:2375
  �| Containers: 7
  �| Reserved CPUs: 0 / 2
  �| Reserved Memory: 0 B / 4.053 GiB
```
