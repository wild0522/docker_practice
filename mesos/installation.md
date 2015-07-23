## Mesos + Marathon �w�˻P�ϥ�

Marathon �O�i�H�� Mesos �@�_��@���@�� framework�A�ΨӹB����[�ʪ����ΡC

### �w��
�@�@�ݭn�w�˥|�زե�Amesos-master�Bmarathon�Bzookeeper �ݭn�w�˨�Ҧ����D�`�I�Amseos-slave �ݭn�w�˨�q�`�I�C

mesos �Q�� zookper �Ӷi��D�`�I���P�B�A�H�αq�`�I�o�{�D�`�I���L�{�C

#### ���X�sĶ

�U�����X
```sh
git clone https://git-wip-us.apache.org/repos/asf/mesos.git
```

�w�˨̿�

```sh
#jdk-7
sudo apt-get update && sudo apt-get install -y openjdk-7-jdk
#autotools
sudo apt-get install -y autoconf libtool
#Mesos dependencies.
sudo apt-get -y install build-essential python-dev python-boto libcurl4-nss-dev libsasl2-dev maven libapr1-dev libsvn-dev
```

�sĶ&�w��
```sh
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

$ mkdir build
$ cd build && ../configure
$ make
$ make check && make install
```

#### [�n�󷽦w��](https://mesosphere.com/downloads/)
�H ubuntu �t�ά��ҡC

�w�� Docker�A���A�حz�A�i�H�Ѧ� [�o��](http://yeasy.gitbooks.io/docker_practice/content/install/index.html)�C

```sh
# Setup
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
CODENAME=$(lsb_release -cs)

# Add the repository
echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | \
  sudo tee /etc/apt/sources.list.d/mesosphere.list

sudo apt-get -y update && sudo apt-get -y install zookeeper mesos marathon
```

#### [��� Docker](https://github.com/sekka1/mesosphere-docker)
�N���p�U�蹳�G

* ZooKeeper�Ghttps://registry.hub.docker.com/u/garland/zookeeper/
* Mesos�Ghttps://registry.hub.docker.com/u/garland/mesosphere-docker-mesos-master/
* Marathon�Ghttps://registry.hub.docker.com/u/garland/mesosphere-docker-marathon/

�䤤 mesos-master �蹳�N�@�� master �M slave �e���ϥΡC

�ɥX���a�������a�}�������ܶq�C
```sh
HOST_IP=10.11.31.7
```

�Ұ� Zookeepr �e���C
```sh
docker run -d \
-p 2181:2181 \
-p 2888:2888 \
-p 3888:3888 \
garland/zookeeper
```

�Ұ� Mesos Master �e���C
```sh
docker run --net="host" \
-p 5050:5050 \
-e "MESOS_HOSTNAME=${HOST_IP}" \
-e "MESOS_IP=${HOST_IP}" \
-e "MESOS_ZK=zk://${HOST_IP}:2181/mesos" \
-e "MESOS_PORT=5050" \
-e "MESOS_LOG_DIR=/var/log/mesos" \
-e "MESOS_QUORUM=1" \
-e "MESOS_REGISTRY=in_memory" \
-e "MESOS_WORK_DIR=/var/lib/mesos" \
-d \
garland/mesosphere-docker-mesos-master
```

�Ұ� Marathon�C
```sh
docker run \
-d \
-p 8080:8080 \
garland/mesosphere-docker-marathon --master zk://${HOST_IP}:2181/mesos --zk zk://${HOST_IP}:2181/marathon
```

�Ұ� Mesos slave �e���C
```sh
docker run -d \
--name mesos_slave_1 \
--entrypoint="mesos-slave" \
-e "MESOS_MASTER=zk://${HOST_IP}:2181/mesos" \
-e "MESOS_LOG_DIR=/var/log/mesos" \
-e "MESOS_LOGGING_LEVEL=INFO" \
garland/mesosphere-docker-mesos-master:latest
```

���U�ӡA�i�H�q�L�X�ݥ��a 8080 �ݤf�Өϥ� Marathon �Ұʥ��ȤF�C


### �t�m����

#### ZooKeepr

ZooKeepr �O�@�Ӥ��G�����Ϊ���դu��A�ΨӺ޲z�h�� Master �`�I�����|�M���E�A��ť�b 2181 �ݤf�C

�t�m���b /etc/zookeeper/conf/ �ؿ��U�C

�����A�n�ק� myid�A��ʬ��C�@�Ӹ`�I���t�@�Ӧۤv�� id�]1-255�����^�C

zoo.cfg �O�D�t�m���A�D�n�ק�p�U���T��]�p�G�A�ҰʤT�� zk �`�I�^�C
```sh
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

�D���W�ݭn�ۤv�����A�æb /etc/hosts ����s�C

�Ĥ@�Ӻݤf�t�d�q�`�I�s����D�`�I���F�ĤG�Ӻݤf�t�d�D�`�I�����|�q�H�C

#### Mesos

Mesos ���q�{�t�m�ؿ����O���G

* /etc/mesos�G�@�P���t�m���A�����䪺�O zk ���F
* /etc/mesos-master�G�D�`�I���t�m�A������Ұ� mesos-master �ɭԪ��q�{�ﶵ�F
* /etc/mesos-slave�G�q�`�I���t�m�A������Ұ� mesos-master �ɭԪ��q�{�ﶵ�C

###### �D�`�I
�����b�Ҧ��`�I�W�ק� /etc/mesos/zk�A�� �D�`�I�� zookeeper �a�}�C��A�Ҧp�G
```sh
zk://ip1:2181,ip2:2181/mesos
```
�Ы� /etc/mesos-master/ip ���A�g�J�D�`�I��ť���a�}�C

�٥i�H�Ы� /etc/mesos-master/cluster ���A�g�J���s���O�W�C

����A�ҰʪA�ȡG
```sh
sudo service mesos-master start
```
��h�ﶵ�i�H�Ѧ�[�o��](http://open.mesosphere.com/reference/mesos-master/)�C

###### �q�`�I

�b�q�`�I�W�A�ק� /etc/mesos-slave/ip ���A�g�J��D�`�I�q�H���a�}�C

����A�ҰʪA�ȡC
```sh
sudo service mesos-slave start
```

��h�ﶵ�i�H�Ѧ�[�o��](http://open.mesosphere.com/reference/mesos-slave/)�C

���ɡA�q�L�s�����X�ݥ��a 5050 �ݤf�A�i�H�ݨ�`�I�H���C

![mesos](../_images/mesos.png)

#### Marathon
�Ұ� marathon �A�ȡC
```sh
sudo service marathon start
```

�Ұʦ��\��A�b mesos �� web�ɭ��� frameworks ���ҭ��U���N��ݨ�W�٬� marathon ���ج[�X�{�C

�P�ɥi�H�q�L�s�����X�� 8080 �ݤf�A�ݨ� marathon ���޲z�ɭ��C

![marathon](../_images/marathon.png)

���ɡA�i�H�q�L�ɭ��Ϊ� REST API �ӳЫؤ@�����ΡAMarathon �|�O�������Ϊ�����B��C
