# �ֳt�W��

�ثe�AKubenetes ����b�h�����ҤU���w�ˡA�]�A���a�D���]Fedora�^�B���A�ȡ]Google GAE�BAWS ���^�C�M�ӳֳ̧t���� Kubernetes ���覡��M�O���a�q�L Docker ���覡�ӱҰʬ����i�{�C

�U�Ϯi�ܤF�b��`�I�ϥ� Docker �ֳt���p�@�M Kubernetes ���ݼ��C

![�b Docker ���Ұ� Kubernetes](../../_images/k8s-singlenode-docker.png)

Kubernetes �̿� Etcd �A�ȨӺ��@�Ҧ��D�`�I�����A�C

## �Ұ� Etcd �A�ȡC
```sh
docker run --net=host -d gcr.io/google_containers/etcd:2.0.9 /usr/local/bin/etcd --addr=127.0.0.1:4001 --bind-addr=0.0.0.0:4001 --data-dir=/var/etcd/data
```

## �ҰʥD�`�I
�Ұ� kubelet�C
```sh
docker run --net=host -d -v /var/run/docker.sock:/var/run/docker.sock  gcr.io/google_containers/hyperkube:v0.17.0 /hyperkube kubelet --api_servers=http://localhost:8080 --v=2 --address=0.0.0.0 --enable_server --hostname_override=127.0.0.1 --config=/etc/kubernetes/manifests
```

## �ҰʪA�ȥN�z
```sh
docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v0.17.0 /hyperkube proxy --master=http://127.0.0.1:8080 --v=2
```

## ���ժ��A
�b���a�X�� 8080 �ݤf�A��������������p�U�����G�G
```sh
$ curl 127.0.0.1:8080
{
  "paths": [
    "/api",
    "/api/v1beta1",
    "/api/v1beta2",
    "/api/v1beta3",
    "/healthz",
    "/healthz/ping",
    "/logs/",
    "/metrics",
    "/static/",
    "/swagger-ui/",
    "/swaggerapi/",
    "/validate",
    "/version"
  ]
}
```

## �d�ݪA��
�Ҧ��A�ȱҰʫ�L�@�|�A�d�ݥ��a��ڹB�檺 Docker �e���A���Ӧ��p�U�X�ӡC
```sh
CONTAINER ID        IMAGE                                        COMMAND                CREATED             STATUS              PORTS               NAMES
ee054db2516c        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube schedule   2 days ago          Up 1 days                               k8s_scheduler.509f29c9_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_e97037f5
3b0f28de07a2        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube apiserve   2 days ago          Up 1 days                               k8s_apiserver.245e44fa_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_6ab5c23d
2eaa44ecdd8e        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube controll   2 days ago          Up 1 days                               k8s_controller-manager.33f83d43_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_1a60106f
30aa7163cbef        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube proxy --   2 days ago          Up 1 days                               jolly_davinci
a2f282976d91        gcr.io/google_containers/pause:0.8.0         "/pause"               2 days ago          Up 2 days                               k8s_POD.e4cc795_k8s-master-127.0.0.1_default_9941e5170b4365bd4aa91f122ba0c061_e8085b1f
c060c52acc36        gcr.io/google_containers/hyperkube:v0.17.0   "/hyperkube kubelet    2 days ago          Up 1 days                               serene_nobel
cc3cd263c581        gcr.io/google_containers/etcd:2.0.9          "/usr/local/bin/etcd   2 days ago          Up 1 days                               happy_turing
```

�o�ǪA�Ȥj�������T���G�D�`�I�A�ȡB�u�@�`�I�A�ȩM�䥦�A�ȡC

### �D�`�I�A��
* apiserver �O��Өt�Ϊ���~���f�A���� RESTful �覡�ѫȤ�ݩM�䥦�ե�եΡF
* scheduler �t�d��귽�i��իסA���t�Y�� pod ��Y�Ӹ`�I�W�F
* controller-manager �t�d�޲z����A�]�A endpoint-controller�]��s�A�ȩM pod �����p�H���^�M replication-controller�]���@�Y�� pod ���ƻs���t�m���ƭȡ^�C

### �u�@�`�I�A��
* kubelet �O�u�@�`�I����ާ@�� agent�A�t�d���骺�e���ͩR�g���޲z�A�ھڱq�ƾڮw��������H���Ӻ޲z�e���A�äW�� pod �B�檬�A���F
* proxy �� pod �W���A�ȴ��ѳX�ݪ��N�z�C

### �䥦�A��
* etcd �O�Ҧ����A���s�x�ƾڮw�F
* `gcr.io/google_containers/pause:0.8.0` �O Kubernetes �Ұʫ�۰� pull �U�Ӫ������蹳�C
