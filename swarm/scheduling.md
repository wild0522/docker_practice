## swarm �ի׵���
swarm����h�ؽի׵����ӿ�ܸ`�I�C�C���bswarm�Ұ�container���ɭԡAswarm�|�ھڿ�ܪ��ի׵����ӿ�ܸ`�I�B��container�C�ثe�������:spread,binpack�Mrandom�C

�b����`swarm manage`�R�O�Ұ� swarm ���s���ɭԥi�H�q�L `--strategy` �Ѽƨӫ��w�A�q�{���Ospread�C

spread�Mbinpack�����|�ھڨC�x�`�I���i��CPU�A���s�H�Υ��b�B�檺containers���ƶq�ӵ��U�Ӹ`�I���šA��random�����A�U�W��q�A�L���|�����󪺭p��A�u�O��ª��H����ܤ@�Ӹ`�I�ӱҰ�container�C�o�ص����@��u���ոեΡC

�ϥ�spread�����Aswarm�|��ܤ@�ӥ��b�B�檺container���ƶq�̤֪����Ӹ`�I�ӹB��container�C�o�ر��p�|�ɭP�Ұʪ�container�|�ɥi�઺���G�b���P�������W�B��A�o�˪��n�B�N�O�p�G���`�I�a�����ɭԤ��|�l���Ӧh��container�C

binpack �h�ۤϡA�o�ر��p�U�Aswarm�|�ɥi�઺��Ҧ����e����b�@�x�`�I�W���B��C�o�ص����|�קK�e���H���ơA�]���L�|�⥼�ϥΪ��������t����j���e���A�a�Ӫ��n�B�N�Oswarm�|�ϥγ̤֪��`�I�B��̦h���e���C

### spread ����
���Ӻt�ܤU spread ���������p�C
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage --strategy=spread file:///tmp/cluster
7609ac2e463f435c271d17887b7d1db223a5d696bf3f47f86925c781c000cb60
ats@sclu083:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
7609ac2e463f        swarm:latest        "/swarm manage --str   6 seconds ago       Up 5 seconds        0.0.0.0:2376->2375/tcp   focused_babbage
```
�T�x�������F83�B��F Swarm���~�A��L�����S���B�����@�Ӯe���A�{�b�b85�o�x�`�I�W���bswarm���s�W�Ұʤ@�Ӯe��
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-1 -d -P redis
2553799f1372b432e9b3311b73e327915d996b6b095a30de3c91a47ff06ce981
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS                          NAMES
2553799f1372        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.84:32770->6379/tcp   084/node-1
```
�Ұʤ@�� redis �e���A�d�ݵ��G
```sh

rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-2 -d -P redis
7965a17fb943dc6404e2c14fb8585967e114addca068f233fcaf60c13bcf2190
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE                            COMMAND                CREATED                  STATUS              PORTS                           NAMES
7965a17fb943        redis:latest   /entrypoint.sh redis   Less than a second ago   Up 1 seconds        192.168.1.124:49154->6379/tcp   124/node-2                  
2553799f1372        redis:latest                     /entrypoint.sh redis   29 minutes ago           Up 4 minutes        192.168.1.84:32770->6379/tcp    084/node-1
```
�A���Ұʤ@�� redis �e���A�d�ݵ��G
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-3 -d -P redis
65e1ed758b53fbf441433a6cb47d288c51235257cf1bf92e04a63a8079e76bee
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE                            COMMAND                CREATED                  STATUS              PORTS                           NAMES
7965a17fb943        redis:latest                     /entrypoint.sh redis   Less than a second ago   Up 4 minutes        192.168.1.227:49154->6379/tcp   124/node-2
65e1ed758b53        redis:latest                     /entrypoint.sh redis   25 minutes ago           Up 17 seconds       192.168.1.83:32770->6379/tcp    083/node-3
2553799f1372        redis:latest                     /entrypoint.sh redis   33 minutes ago           Up 8 minutes        192.168.1.84:32770->6379/tcp    084/node-1
```
�i�H�ݨ�T�Ӯe�����O���G�b���P���`�I�W�����C

### binpack ����
�{�b�Ӭݬ�binpack�����U�����p�C�b083�W������R�O�G
```sh
rio@083:~$ sudo docker run -d -p 2376:2375 -v $(pwd)/cluster:/tmp/cluster swarm manage --strategy=binpack  file:///tmp/cluster
f1c9affd5a0567870a45a8eae57fec7c78f3825f3a53fd324157011aa0111ac5
```

�{�b�b���s���ҰʤT�� redis �e���A�d�ݤ��G���p�G
```sh
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-1 -d -P redis
18ceefa5e86f06025cf7c15919fa64a417a9d865c27d97a0ab4c7315118e348c
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-2 -d -P redis
7e778bde1a99c5cbe4701e06935157a6572fb8093fe21517845f5296c1a91bb2
rio@085:~$ sudo docker -H 192.168.1.83:2376 run --name node-3 -d -P redis
2195086965a783f0c2b2f8af65083c770f8bd454d98b7a94d0f670e73eea05f8
rio@085:~$ sudo docker -H 192.168.1.83:2376 ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS                          NAMES
2195086965a7        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.83:32773->6379/tcp   083/node-3
7e778bde1a99        redis:latest        /entrypoint.sh redis   24 minutes ago      Up Less than a second   192.168.1.83:32772->6379/tcp   083/node-2
18ceefa5e86f        redis:latest        /entrypoint.sh redis   25 minutes ago      Up 22 seconds           192.168.1.83:32771->6379/tcp   083/node-1
```

�i�H�ݨ�A�Ҧ����e�����O���G�b�P�@�Ӹ`�I�W�B�檺�C