##�u�@�A�B��

��h���ɭԡA�ݭn�� Docker �e���b��x�H�u�@�A�]Daemonized�^�Φ��B��C���ɡA�i�H�q�L�K�[ `-d` �Ѽƨӹ�{�C

�Ҧp�U�����R�O�|�b��x�B��e���C
```
$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147
```

�e���Ұʫ�|��^�@�Ӱߤ@�� id�A�]�i�H�q�L `docker ps` �R�O�Ӭd�ݮe���H���C
```
$ sudo docker ps
CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
1e5535038e28  ubuntu:14.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        insane_babbage
```
�n����e������X�H���A�i�H�q�L `docker logs` �R�O�C
```
$ sudo docker logs insane_babbage
hello world
hello world
hello world
. . .
```
