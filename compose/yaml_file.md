## YAML �ҪO���

�q�{���ҪO���O `docker-compose.yml`�A�䤤�w�q���C�ӪA�ȳ������q�L `image` ���O���w�蹳�� `build` ���O�]�ݭn Dockerfile�^�Ӧ۰ʺc�ءC

�䥦�j�������O���� `docker run` ���������C

�p�G�ϥ� `build` ���O�A�b `Dockerfile` ���]�m���ﶵ(�Ҧp�G`CMD`, `EXPOSE`, `VOLUME`, `ENV` ��) �N�|�۰ʳQ����A�L�ݦb `docker-compose.yml` ���A���]�m�C

### `image`

���w���蹳�W�٩��蹳 ID�C�p�G�蹳�b���a���s�b�A`Compose` �N�|���թԥh�o���蹳�C

�Ҧp�G
```sh
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

### `build`

���w `Dockerfile` �Ҧb��󧨪����|�C `Compose` �N�|�Q�Υ��۰ʺc�سo���蹳�A�M��ϥγo���蹳�C

```
build: /path/to/build/dir
```

### `command`

�л\�e���Ұʫ��q�{���檺�R�O�C

```sh
command: bundle exec thin -p 3000
```

### `links`

�챵��䥦�A�Ȥ����e���C�ϥΪA�ȦW�١]�P�ɧ@���O�W�^�ΪA�ȦW�١G�A�ȧO�W `�]SERVICE:ALIAS�^` �榡���i�H�C

```sh
links:
 - db
 - db:database
 - redis
```

�ϥΪ��O�W�N�|�۰ʦb�A�Ȯe������ `/etc/hosts` �̳ЫءC�Ҧp�G

```sh
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

�����������ܶq�]�N�Q�ЫءC

### `external_links`
�챵�� docker-compose.yml �~�����e���A�Ʀ� �ëD `Compose` �޲z���e���C�ѼƮ榡�� `links` �����C

```
external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
```


### `ports`

���S�ݤf�H���C

�ϥαJ�D�G�e�� `�]HOST:CONTAINER�^`�榡�Ϊ̶ȶȫ��w�e�����ݤf�]�J�D�N�|�H����ܺݤf�^���i�H�C

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

*���G��ϥ� `HOST:CONTAINER` �榡�ӬM�g�ݤf�ɡA�p�G�A�ϥΪ��e���ݤf�p�� 60 �A�i��|�o����~�o���G�A�]�� `YAML` �N�|�ѪR `xx:yy` �o�ؼƦr�榡�� 60 �i��C�ҥH��ĳ�ĥΦr�Ŧ�榡�C*


### `expose`

���S�ݤf�A�����M�g��J�D���A�u�Q�s�����A�ȳX�ݡC

�ȥi�H���w�����ݤf���Ѽ�

```sh
expose:
 - "3000"
 - "8000"
```

### `volumes`

���������|�]�m�C�i�H�]�m�J�D�����| �]`HOST:CONTAINER`�^ �Υ[�W�X�ݼҦ� �]`HOST:CONTAINER:ro`�^�C

```sh
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

### `volumes_from`

�q�t�@�ӪA�ȩήe�����������Ҧ����C

```sh
volumes_from:
 - service_name
 - container_name
```

### `environment`

�]�m�����ܶq�C�A�i�H�ϥμƲթΦr���خ榡�C

�u���w�W�٪��ܶq�|�۰�������b Compose �D���W���ȡA�i�H�ΨӨ���S�����n���ƾڡC

```
environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

### `env_file`
�q�����������ܶq�A�i�H����W�������|�ΦC��C

�p�G�q�L `docker-compose -f FILE` ���w�F�ҪO���A�h `env_file` �����|�|���ҪO�����|�C

�p�G���ܶq�W�ٻP `environment` ���O�Ĭ�A�h�H��̬��ǡC

```sh
env_file: .env

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```

�����ܶq��󤤨C�@�楲���ŦX�榡�A��� `#` �}�Y��������C

```sh
# common.env: Set Rails/Rack environment
RACK_ENV=development
```

### `extends`
���w�����A�ȶi���X�i�C�Ҧp�ڭ̤w�g���F�@�� webapp �A�ȡA�ҪO��� `common.yml`�C
```sh
# common.yml
webapp:
  build: ./webapp
  environment:
    - DEBUG=false
    - SEND_EMAILS=false
```

�s�g�@�ӷs�� `development.yml` ���A�ϥ� `common.yml` ���� webapp �A�ȶi���X�i�C
```sh
# development.yml
web:
  extends:
    file: common.yml
    service: webapp
  ports:
    - "8000:8000"
  links:
    - db
  environment:
    - DEBUG=true
db:
  image: postgres
```
��̷|�۰��~�� common.yml ���� webapp �A�Ȥά������`�ܶq�C


### `net`

�]�m�����Ҧ��C�ϥΩM `docker client` �� `--net` �ѼƤ@�˪��ȡC

```sh
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

### `pid`
��D���t�Φ@�ɶi�{�R�W�Ŷ��C���}�ӿﶵ���e���i�H�ۤ��q�L�i�{ ID �ӳX�ݩM�ާ@�C

```sh
pid: "host"
```

### `dns`

�t�m DNS �A�Ⱦ��C�i�H�O�@�ӭȡA�]�i�H�O�@�ӦC��C

```sh
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

### `cap_add, cap_drop`
�K�[�Ω��e���� Linux ��O�]Capabiliity�^�C
```sh
cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN
```

### `dns_search`

�t�m DNS �j����C�i�H�O�@�ӭȡA�]�i�H�O�@�ӦC��C

```sh
dns_search: example.com
dns_search:
  - domain1.example.com
  - domain2.example.com
```

### `working_dir, entrypoint, user, hostname, domainname, mem_limit, privileged, restart, stdin_open, tty, cpu_shares`

�o�ǳ��O�M `docker run` ������ﶵ�����C

```
cpu_shares: 73

working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true

restart: always

stdin_open: true
tty: true
```
