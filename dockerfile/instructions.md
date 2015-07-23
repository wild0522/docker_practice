## ���O
���O���@��榡�� `INSTRUCTION arguments`�A���O�]�A `FROM`�B`MAINTAINER`�B`RUN` ���C

### FROM
�榡�� `FROM <image>`��`FROM <image>:<tag>`�C

�Ĥ@�����O������ `FROM` ���O�C�åB�A�p�G�b�P�@��Dockerfile���Ыئh���蹳�ɡA�i�H�ϥΦh�� `FROM` ���O�]�C���蹳�@���^�C

### MAINTAINER
�榡�� `MAINTAINER <name>`�A���w���@�̫H���C

### RUN
�榡�� `RUN <command>` �� `RUN ["executable", "param1", "param2"]`�C

�e�̱N�b shell �׺ݤ��B��R�O�A�Y `/bin/sh -c`�F��̫h�ϥ� `exec` ����C���w�ϥΨ䥦�׺ݥi�H�q�L�ĤG�ؤ覡��{�A�Ҧp `RUN ["/bin/bash", "-c", "echo hello"]`�C

�C�� `RUN` ���O�N�b��e�蹳��¦�W������w�R�O�A�ô��欰�s���蹳�C��R�O�����ɥi�H�ϥ� `\` �Ӵ���C

### CMD
����T�خ榡
* `CMD ["executable","param1","param2"]` �ϥ� `exec` ����A���ˤ覡�F
* `CMD command param1 param2` �b `/bin/sh` ������A���ѵ��ݭn�椬�����ΡF
* `CMD ["param1","param2"]` ���ѵ� `ENTRYPOINT` ���q�{�ѼơF


���w�Ұʮe���ɰ��檺�R�O�A�C�� Dockerfile �u�঳�@�� `CMD` �R�O�C�p�G���w�F�h���R�O�A�u���̫�@���|�Q����C

�p�G�Τ�Ұʮe���ɭԫ��w�F�B�檺�R�O�A�h�|�л\�� `CMD` ���w���R�O�C

### EXPOSE
�榡�� `EXPOSE <port> [<port>...]`�C

�i�D Docker �A�Ⱥݮe�����S���ݤf���A�Ѥ��pô�ΨϥΡC�b�Ұʮe���ɻݭn�q�L -P�ADocker �D���|�۰ʤ��t�@�Ӻݤf��o����w���ݤf�C

### ENV
�榡�� `ENV <key> <value>`�C
���w�@�������ܶq�A�|�Q���� `RUN` ���O�ϥΡA�æb�e���B��ɫO���C

�Ҧp
```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && �K
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```

### ADD
�榡�� `ADD <src> <dest>`�C

�өR�O�N�ƻs���w�� `<src>` ��e������ `<dest>`�C
�䤤 `<src>` �i�H�ODockerfile�Ҧb�ؿ����@�Ӭ۹���|�F�]�i�H�O�@�� URL�F�٥i�H�O�@�� tar ���]�۰ʸ������ؿ��^�C

### COPY
�榡�� `COPY <src> <dest>`�C

�ƻs���a�D���� `<src>`�]�� Dockerfile �Ҧb�ؿ����۹���|�^��e������ `<dest>`�C

��ϥΥ��a�ؿ������ؿ��ɡA���˨ϥ� `COPY`�C

### ENTRYPOINT
��خ榡�G
* `ENTRYPOINT ["executable", "param1", "param2"]`
* `ENTRYPOINT command param1 param2`�]shell������^�C

�t�m�e���Ұʫ���檺�R�O�A�åB���i�Q `docker run` ���Ѫ��Ѽ��л\�C

�C�� Dockerfile ���u�঳�@�� `ENTRYPOINT`�A����w�h�ӮɡA�u���̫�@�Ӱ_�ġC

### VOLUME
�榡�� `VOLUME ["/data"]`�C

�Ыؤ@�ӥi�H�q���a�D���Ψ�L�e�������������I�A�@��ΨӦs��ƾڮw�M�ݭn�O�����ƾڵ��C

### USER
�榡�� `USER daemon`�C

���w�B��e���ɪ��Τ�W�� UID�A���� `RUN` �]�|�ϥΫ��w�Τ�C

��A�Ȥ��ݭn�޲z���v���ɡA�i�H�q�L�өR�O���w�B��Τ�C�åB�i�H�b���e�Ыةһݭn���Τ�A�Ҧp�G`RUN groupadd -r postgres && useradd -r -g postgres postgres`�C�n�{������޲z���v���i�H�ϥ� `gosu`�A�Ӥ����� `sudo`�C

### WORKDIR
�榡�� `WORKDIR /path/to/workdir`�C

������ `RUN`�B`CMD`�B`ENTRYPOINT` ���O�t�m�u�@�ؿ��C

�i�H�ϥΦh�� `WORKDIR` ���O�A����R�O�p�G�ѼƬO�۹���|�A�h�|��󤧫e�R�O���w�����|�C�Ҧp
```
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
�h�̲׸��|�� `/a/b/c`�C

### ONBUILD
�榡�� `ONBUILD [INSTRUCTION]`�C

�t�m��ҳЫت��蹳�@���䥦�s�Ы��蹳����¦�蹳�ɡA�Ұ��檺�ާ@���O�C

�Ҧp�ADockerfile �ϥΦp�U�����e�ЫؤF�蹳 `image-A`�C
```
[...]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[...]
```

�p�G��� image-A �Ыطs���蹳�ɡA�s��Dockerfile���ϥ� `FROM image-A`���w��¦�蹳�ɡA�|�۰ʰ��� `ONBUILD` ���O���e�A������b�᭱�K�[�F������O�C
```
FROM image-A

#Automatically run the following
ADD . /app/src
RUN /usr/local/bin/python-build --dir /app/src
```

�ϥ� `ONBUILD` ���O���蹳�A���˦b���Ҥ������A�Ҧp `ruby:1.9-onbuild`�C

