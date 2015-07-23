##fig.yml �Ѧ�

�C�Ӧb `fig.yml` �w�q���A�ȳ��ݭn���w�@���蹳���蹳���c�ؤ��e�C�� `docker run` ���R�O��@�ˡA�䥦���e�O�i�諸�C

`docker run` �b `Dockerfile` ���]�m���ﶵ(�Ҧp�G`CMD`, `EXPOSE`, `VOLUME`, `ENV`) �@���w�g���Ѫ��q�{�]�m - �A���ݭn�b `fig.yml` �����s�]�m�C

`image`

�o�̥i�H�]�m�����ҩ��蹳ID���@�����C���i�H�O���a���A�]�i�H�O���{�� - �p�G�蹳�b���a���s�b�A`Fig` �N�|���թԥh�o���蹳�C

```
image: ubuntu
image: orchardup/postgresql
image: a4bc65fd
```

`build`

���w `Dockerfile` �Ҧb��󧨪����|�C `Fig` �N�|�c�سo���蹳�õ����ͦ��@�ӦW�r�A�M��ϥγo���蹳�C

```
build: /path/to/build/dir
```

`command`

�л\�q�{���R�O�C

```
command: bundle exec thin -p 3000
```

`links`

�b�䥦���A�Ȥ��s���e���C�ϥΪA�ȦW�١]�g�`�]�@���O�W�^�ΪA�ȦW�٥[�A�ȧO�W `�]SERVICE:ALIAS�^` ���i�H�C

```
links:
 - db
 - db:database
 - redis
```

�i�H�b�A�Ȫ��e������ `/etc/hosts` �̳ЫاO�W�C�Ҧp�G

```
172.17.2.186  db
172.17.2.186  database
172.17.2.187  redis
```

�����ܶq�]�N�Q�Ы� - �Ӹ`�d�������ܶq�Ѧҳ��`�C

`ports`

���S�ݤf�C�ϥαJ�D�M�e�� `�]HOST:CONTAINER�^` �Ϊ̶ȶȮe�����ݤf�]�J�D�N�|�H����ܺݤf�^���i�H�C

���G��ϥ� `HOST:CONTAINER` �榡�ӬM�g�ݤf�ɡA�p�G�A�ϥΪ��e���ݤf�p��60�A�i��|�o����~�o���G�A�]�� `YAML` �N�|�ѪR `xx:yy` �o�ؼƦr�榡��60�i��C�ҥH�ڭ̫�ĳ�Φr�ū��w�A�o�ݤf�M�g�C

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

`expose`

���S���o�G��J�D�����ݤf - ���̥u�Q�s�����A�ȳX�ݡC�ȶȤ������ݤf�i�H�Q���w�C

```
expose:
 - "3000"
 - "8000"
```

`volumes`

���������|�]�m�C�i�H�]�m�J�D�����| `�]HOST:CONTAINER�^` �γX�ݼҦ� `�]HOST:CONTAINER:ro�^` �C

```
volumes:
 - /var/lib/mysql
 - cache/:/tmp/cache
 - ~/configs:/etc/configs/:ro
```

`volumes_from`

�q�t�@�ӪA�ȩήe�������Ҧ����C

```
volumes_from:
 - service_name
 - container_name
```

`environment`

�]�m�����ܶq�C�A�i�H�ϥμƲթΦr���خ榡�C

�����ܶq�b�B�� `Fig` �������W�Q�ѪR���@��key�C�����U��w���M���w���J�D�ȡC

```
environment:
  RACK_ENV: development
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SESSION_SECRET
```

`net`

�]�m�����Ҧ��C�ϥΩM `docker client` �� `--net` �ѼƤ@�˪��ȡC

```
net: "bridge"
net: "none"
net: "container:[name or id]"
net: "host"
```

`dns`

�t�mDNS�A�Ⱦ��C���i�H�O�@�ӭȡA�]�i�H�O�@�ӦC��C

```
dns: 8.8.8.8
dns:
  - 8.8.8.8
  - 9.9.9.9
```

`working_dir, entrypoint, user, hostname, domainname, mem_limit, privileged`

�o�ǳ��O�M `docker run` �������@�ӭȡC

```
working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true
```
