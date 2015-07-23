## �p���ܮw

���ɭԨϥ� Docker Hub �o�˪����@�ܮw�i�ण��K�A�Τ�i�H�Ыؤ@�ӥ��a�ܮw�Ѩp�H�ϥΡC

���`���Цp��ϥΥ��a�ܮw�C

`docker-registry` �O�x�责�Ѫ��u��A�i�H�Ω�c�بp�����蹳�ܮw�C
### �w�˹B�� docker-registry
#### �e���B��
�b�w�ˤF Docker ��A�i�H�q�L����x�� registry �蹳�ӹB��C
```
$ sudo docker run -d -p 5000:5000 registry
```
�o�N�ϥΩx�誺 registry �蹳�ӱҰʥ��a���p���ܮw�C
�Τ�i�H�q�L���w�ѼƨӰt�m�p���ܮw��m�A�Ҧp�t�m�蹳�s�x�� Amazon S3 �A�ȡC
```
$ sudo docker run \
         -e SETTINGS_FLAVOR=s3 \
         -e AWS_BUCKET=acme-docker \
         -e STORAGE_PATH=/registry \
         -e AWS_KEY=AKIAHSHB43HS3J92MXZ \
         -e AWS_SECRET=xdDowwlK7TJajV1Y7EoOZrmuPEJlHYcNP2k4j49T \
         -e SEARCH_BACKEND=sqlalchemy \
         -p 5000:5000 \
         registry
````
���~�A�٥i�H���w���a���|�]�p `/home/user/registry-conf` �^�U���t�m���C
```
$ sudo docker run -d -p 5000:5000 -v /home/user/registry-conf:/registry-conf -e DOCKER_REGISTRY_CONFIG=/registry-conf/config.yml registry
```
�q�{���p�U�A�ܮw�|�Q�Ыئb�e���� `/tmp/registry` �U�C�i�H�q�L `-v` �ѼƨӱN�蹳���s��b���a�����w���|�C
�Ҧp�U�����Ҥl�N�W�Ǫ��蹳��� `/opt/data/registry` �ؿ��C
```
$ sudo docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry
```

#### ���a�w��
��� Ubuntu �� CentOS ���o�檩�A�i�H�����q�L���w�ˡC
* Ubuntu
```
$ sudo apt-get install -y build-essential python-dev libevent-dev python-pip liblzma-dev
$ sudo pip install docker-registry
```
* CentOS
```
$ sudo yum install -y python-devel libevent-devel python-pip gcc xz-devel
$ sudo python-pip install docker-registry
```

�]�i�H�q [docker-registry](https://github.com/docker/docker-registry) ���ؤU�����X�i��w�ˡC
```
$ sudo apt-get install build-essential python-dev libevent-dev python-pip libssl-dev liblzma-dev libffi-dev
$ git clone https://github.com/docker/docker-registry.git
$ cd docker-registry
$ sudo python setup.py install
```
�M��ק�t�m���A�D�n�ק� dev �ҪO�q�� `storage_path` �쥻�a���s�x�ܮw�����|�C
```
$ cp config/config_sample.yml config/config.yml
```
����Ұ� Web �A�ȡC
```
$ sudo gunicorn -c contrib/gunicorn.py docker_registry.wsgi:application
```
�Ϊ�
```
$ sudo gunicorn --access-logfile - --error-logfile - -k gevent -b 0.0.0.0:5000 -w 4 --max-requests 100 docker_registry.wsgi:application
```
���ɨϥ� curl �X�ݥ��a�� 5000 �ݤf�A�ݨ��X docker-registry �������H�������B�榨�\�C

*���G`config/config_sample.yml` ���O�ܨҰt�m���C

###�b�p���ܮw�W�ǡB�U���B�j���蹳
�Ыئn�p���ܮw����A�N�i�H�ϥ� `docker tag` �ӼаO�@���蹳�A�M����e����ܮw�A�O�������W�N�i�H�U���U�ӤF�C�Ҧp�p���ܮw�a�}�� `192.168.7.26:5000`�C

���b�����d�ݤw�����蹳�C
```
$ sudo docker images
REPOSITORY                        TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu                            latest              ba5877dc9bec        6 weeks ago         192.7 MB
ubuntu                            14.04               ba5877dc9bec        6 weeks ago         192.7 MB
```

�ϥ�`docker tag` �N `ba58` �o���蹳�аO�� `192.168.7.26:5000/test`�]�榡�� `docker tag IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]`�^�C
```
$ sudo docker tag ba58 192.168.7.26:5000/test
root ~ # docker images
REPOSITORY                        TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu                            14.04               ba5877dc9bec        6 weeks ago         192.7 MB
ubuntu                            latest              ba5877dc9bec        6 weeks ago         192.7 MB
192.168.7.26:5000/test            latest              ba5877dc9bec        6 weeks ago         192.7 MB
```
�ϥ� `docker push` �W�ǼаO���蹳�C
```
$ sudo docker push 192.168.7.26:5000/test
The push refers to a repository [192.168.7.26:5000/test] (len: 1)
Sending image list
Pushing repository 192.168.7.26:5000/test (1 tags)
Image 511136ea3c5a already pushed, skipping
Image 9bad880da3d2 already pushed, skipping
Image 25f11f5fb0cb already pushed, skipping
Image ebc34468f71d already pushed, skipping
Image 2318d26665ef already pushed, skipping
Image ba5877dc9bec already pushed, skipping
Pushing tag for rev [ba5877dc9bec] on {http://192.168.7.26:5000/v1/repositories/test/tags/latest}
```
�� curl �d�ݭܮw�����蹳�C
```
$ curl http://192.168.7.26:5000/v1/search
{"num_results": 7, "query": "", "results": [{"description": "", "name": "library/miaxis_j2ee"}, {"description": "", "name": "library/tomcat"}, {"description": "", "name": "library/ubuntu"}, {"description": "", "name": "library/ubuntu_office"}, {"description": "", "name": "library/desktop_ubu"}, {"description": "", "name": "dockerfile/ubuntu"}, {"description": "", "name": "library/test"}]}
```
�o�̥i�H�ݨ� `{"description": "", "name": "library/test"}`�A����蹳�w�g�Q���\�W�ǤF�C

�{�b�i�H��t�~�@�x�����h�U���o���蹳�C
```
$ sudo docker pull 192.168.7.26:5000/test
Pulling repository 192.168.7.26:5000/test
ba5877dc9bec: Download complete
511136ea3c5a: Download complete
9bad880da3d2: Download complete
25f11f5fb0cb: Download complete
ebc34468f71d: Download complete
2318d26665ef: Download complete
$ sudo docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
192.168.7.26:5000/test             latest              ba5877dc9bec        6 weeks ago         192.7 MB
```

�i�H�ϥ� [�o�Ӹ}��](https://github.com/yeasy/docker_practice/raw/master/_local/push_images.sh) ��q�W�ǥ��a���蹳����U�A�Ⱦ����A�q�{�O���a���U�A�Ⱦ� `127.0.0.1:5000`�C�Ҧp�G
```
$ wget https://github.com/yeasy/docker_practice/raw/master/_local/push_images.sh; sudo chmod a+x push_images.sh
$ ./push_images.sh ubuntu:latest centos:centos7
The registry server is 127.0.0.1
Uploading ubuntu:latest...
The push refers to a repository [127.0.0.1:5000/ubuntu] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/ubuntu (1 tags)
Image 511136ea3c5a already pushed, skipping
Image bfb8b5a2ad34 already pushed, skipping
Image c1f3bdbd8355 already pushed, skipping
Image 897578f527ae already pushed, skipping
Image 9387bcc9826e already pushed, skipping
Image 809ed259f845 already pushed, skipping
Image 96864a7d2df3 already pushed, skipping
Pushing tag for rev [96864a7d2df3] on {http://127.0.0.1:5000/v1/repositories/ubuntu/tags/latest}
Untagged: 127.0.0.1:5000/ubuntu:latest
Done
Uploading centos:centos7...
The push refers to a repository [127.0.0.1:5000/centos] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/centos (1 tags)
Image 511136ea3c5a already pushed, skipping
34e94e67e63a: Image successfully pushed
70214e5d0a90: Image successfully pushed
Pushing tag for rev [70214e5d0a90] on {http://127.0.0.1:5000/v1/repositories/centos/tags/centos7}
Untagged: 127.0.0.1:5000/centos:centos7
Done
```
