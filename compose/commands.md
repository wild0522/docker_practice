## Compose �R�O����

�j�����R�O���i�H�B��b�@�өΦh�ӪA�ȤW�C�p�G�S���S�O�������A�R�O�h���Φb���ةҦ����A�ȤW�C

���� `docker-compose [COMMAND] --help` �d�ݨ���Y�өR�O���ϥλ����C

�򥻪��ϥή榡�O
```sh
docker-compose [options] [COMMAND] [ARGS...]
```

## �ﶵ

* `--verbose` ��X��h�ոիH���C
* `--version` ���L�����ðh�X�C
* `-f, --file FILE` �ϥίS�w�� compose �ҪO���A�q�{�� `docker-compose.yml`�C
* `-p, --project-name NAME` ���w���ئW�١A�q�{�ϥΥؿ��W�١C

## �R�O

### `build`

�c�ةέ��s�c�تA�ȡC

�A�Ȥ@���c�ث�A�N�|�a�W�@�ӼаO�W�A�Ҧp web_db�C

�i�H�H�ɦb���إؿ��U�B�� `docker-compose build` �ӭ��s�c�تA�ȡC

### `help`

��o�@�өR�O�����U�C

### `kill`

�q�L�o�e `SIGKILL` �H���ӱj���A�Ȯe���C����q�L�Ѽƨӫ��w�o�e���H���A�Ҧp
```sh
$ docker-compose kill -s SIGINT
```

### `logs`

�d�ݪA�Ȫ���X�C

### `port`

���L�j�w�����@�ݤf�C

### `ps`

�C�X�Ҧ��e���C

### `pull`

�Ԩ��A���蹳�C

### `rm`

�R������A�Ȯe���C

### `run`

�b�@�ӪA�ȤW����@�өR�O�C

�Ҧp�G

```
$ docker-compose run ubuntu ping docker.com
```

�N�|�Ұʤ@�� ubuntu �A�ȡA���� `ping docker.com` �R�O�C

�q�{���p�U�A�Ҧ����p���A�ȱN�|�۰ʳQ�ҰʡA���D�o�ǪA�Ȥw�g�b�B�椤�C

�өR�O�����Ұʮe����B����w���R�O�A�������B�챵�������N�|���Ӵ���ЫءC

��Ӥ��P�I�G
* ���w�R�O�N�|�л\�즳���۰ʹB��R�O�F
* ���|�۰ʳЫغݤf�A�H�קK�Ĭ�C

�p�G���Ʊ�۰ʱҰ����p���e���A�i�H�ϥ� `--no-deps` �ﶵ�A�Ҧp

```
$ docker-compose run --no-deps web python manage.py shell
```

�N���|�Ұ� web �e�������p���䥦�e���C

### `scale`

�]�m�P�@�ӪA�ȹB�檺�e���ӼơC

�q�L `service=num` ���Ѽƨӳ]�m�ƶq�C�Ҧp�G

```
$ docker-compose scale web=2 worker=3
```

### `start`

�Ұʤ@�Ӥw�g�s�b���A�Ȯe���C

### `stop`

����@�Ӥw�g�B�檺�e���A�����R�����C�q�L `docker-compose start` �i�H�A���Ұʳo�Ǯe���C

### `up`

�c�ءA�]���s�^�ЫءA�ҰʡA�챵�@�ӪA�Ȭ������e���C

�챵���A�ȳ��N�|�ҰʡA���D�L�̤w�g�B��C

�q�{���p�A `docker-compose up` �N�|��X�Ҧ��e������X�A�åB�h�X�ɡA�Ҧ��e���N�|����C

�p�G�ϥ� `docker-compose up -d` �A�N�|�b��x�ҰʨùB��Ҧ����e���C

�q�{���p�A�p�G�ӪA�Ȫ��e���w�g�s�b�A `docker-compose up` �N�|����ù��խ��s�ЫإL�̡]�O���ϥ� `volumes-from` ���������^�A�H�O�� `docker-compose.yml` ���ק�ͮġC�p�G�A���Q�e���Q����í��s�ЫءA�i�H�ϥ� `docker-compose up --no-recreate`�C�p�G�ݭn���ܡA�o�˱N�|�Ұʤw�g����e���C

## �����ܶq

�����ܶq�i�H�ΨӰt�m Compose ���欰�C

�H`DOCKER_`�}�Y���ܶq�M�ΨӰt�m Docker �R�O��Ȥ�ݪ��ϥΤ@�ˡC�p�G�ϥ� boot2docker , `$(boot2docker shellinit)` �N�|�]�m���̬����T���ȡC

### `COMPOSE_PROJECT_NAME`

�]�m�q�L Compose �Ұʪ��C�@�Ӯe���e�K�[�����ئW�١A�q�{�O��e�u�@�ؿ����W�r�C

### `COMPOSE_FILE`

�]�m�n�ϥΪ� `docker-compose.yml` �����|�C�q�{���|�O��e�u�@�ؿ��C

### `DOCKER_HOST`

�]�m Docker daemon ���a�}�C�q�{�ϥ� `unix:///var/run/docker.sock`�A�P Docker �Ȥ�ݱĥΪ��q�{�Ȥ@�P�C

### `DOCKER_TLS_VERIFY`

�p�G�]�m�����šA�h�P Docker daemon �椬�q�L TLS �i��C

### `DOCKER_CERT_PATH`

�t�m TLS �q�H�һݭn�����ҡ]`ca.pem`�B`cert.pem` �M `key.pem`�^��󪺸��|�A�q�{�O `~/.docker` �C
