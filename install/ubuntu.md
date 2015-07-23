## Ubuntu �t�C�w�� Docker

### �q�L�t�Φ۱a�]�w��
Ubuntu 14.04 �����t�Τ��w�g�۱a�F Docker �]�A�i�H�����w�ˡC
```
$ sudo apt-get update
$ sudo apt-get install -y docker.io
$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
$ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io
```

�p�G�ϥξާ@�t�Φ۱a�]�w�� Docker�A�ثe�w�˪������O����ª� 0.9.1�C �n�w�˧�s�������A�i�H�q�L�ϥ� Docker �����覡�C

### �q�LDocker���w�˳̷s����
�n�w�˳̷s�� Docker �����A�����ݭn�w�� apt-transport-https ����A����q�L�K�[���Ӧw�ˡC
```
$ sudo apt-get install apt-transport-https
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
$ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
$ sudo apt-get update
$ sudo apt-get install lxc-docker
```

### 14.04 ���e����
�p�G�O���C������ Ubuntu �t�ΡA�ݭn����s���֡C
```
$ sudo apt-get update
$ sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
$ sudo reboot
```
�M�᭫�ƤW�����B�J�Y�i�C

�w�ˤ���Ұ� Docker �A�ȡC
```
$ sudo service docker start
```
