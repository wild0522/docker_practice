## �w��
Docker Machine �i�H�b�h�ؾާ@�t�Υ��x�W�w�ˡA�]�A Linux�BMac OS�A�H�� Windows�C


### Linux/Mac OS
�b Linux/Mac OS �W���w�ˤQ��²��A���˱q [�x�� Release �w](https://github.com/docker/machine/releases) �����U���sĶ�n���G�i����Y�i�C

�Ҧp�A�b Linux 64 ��t�ΤW�����U���������G�i��]�C
```sh
$ sudo curl -L https://github.com/docker/machine/releases/download/v0.3.1-rc1/docker-machine_linux-amd64 > /usr/local/bin/docker-machine
$ chmod +x /usr/local/bin/docker-machine
```

������A�d�ݪ����H���A���ҹB�楿�`�C
```sh
$ docker-machine -v
docker-machine version 0.3.1-rc1 (993f2db)
```

### Windows
Windows �U���n�����@�ǡA�����ݭn�w�� [msysgit](https://msysgit.github.io/)�C

msysgit �O Windows �U�� git �Ȥ�ݳn��]�A�|�������� Linux �U���@�ǰ򥻪��u��A�Ҧp ssh ���C

�w�ˤ���A�Ұ� msysgit ���R�O��ɭ��A���M�q�L�U���G�i��]�i��w�ˡA�ݭn�U�� docker �Ȥ�ݩM docker-machine�C

```sh
$ curl -L https://get.docker.com/builds/Windows/x86_64/docker-latest.exe > /bin/docker

$ curl -L https://github.com/docker/machine/releases/download/v0.3.1-rc1/docker-machine_windows-amd64.exe > /bin/docker-machine
```
