##Fig�Ȥ�ݰѦ�

�j�����R�O���i�H�B��b�@�өΦh�ӪA�ȤW�C�p�G�S���S�O�������A�o�өR�O�h�i�H���Φb�Ҧ����A�ȤW�C

���� `fig [COMMAND] --help` �d�ݩҦ����ϥλ����C

###�ﶵ

`--verbose`

��ܧ�h�H���C

`--version`

���L�����ðh�X�C

`-f, --file FILE`

�ϥίS�w��Fig���A�q�{�ϥ�fig.yml�C

`-p, --project-name NAME`

�ϥίS�w�����ئW�١A�q�{�ϥΤ�󧨦W�١C

###�R�O

`build`

�c�ةέ��s�c�تA�ȡC

�A�Ȥ@���c�ث�A�N�|�аO��project_service�A�Ҧpfigtest_db�C
�p�G�ק�A�Ȫ� `Dockerfile` �κc�إؿ��H���A�A�i�H�B�� `fig build` �ӭ��s�c�ءC

`help`

��o�@�өR�O�����U�C

`kill`

�j���A�Ȯe���C

`logs`

�d�ݪA�Ȫ���X�C

`port`

���L�ݤf�j�w�����@�ݤf�C

`ps`

�C�X�Ҧ��e���C

`pull`

�Ԩ��A���蹳�C

`rm`

�R������A�Ȯe���C

`run`

�b�@�ӪA�ȤW����@�өR�O�C

�Ҧp�G

```
$ fig run web python manage.py shell
```

�q�{���p�U�A�챵���A�ȱN�|�ҰʡA���D�o�ǪA�Ȥw�g�b�B�椤�C

�@���ʩR�O�|�b�ϥλP�A�Ȫ����q�e���ۦP���t�m���s�e�����}�l�B��A�M����B�챵�������N�|���Ӵ���ЫءC
�P���q�e���ߤ@�����P�N�O�A�o�өR�O�N�|�л\�즳���R�O�A�p�G�ݤf���Ĭ�h���|�ЫءC

�챵�٥i�H�b�@���ʩR�O�M���ӪA�Ȫ���L�e�����ЫءA�M��A�i�H���U���@�˶i��@�Ǿާ@�G

```
$ fig run db psql -h db -U docker
```

�p�G�A���Ʊ�b����@���ʩR�O�ɱҰ��챵���e���A�i�H���w--no-deps�ﶵ�G

```
$ fig run --no-deps web python manage.py shell
```

`scale`

�]�m�@�ӪA�Ȼݭn�B�檺�e���ӼơC

�q�Lservice=num���Ѽƨӳ]�m�ƶq�C�Ҧp�G

```
$ fig scale web=2 worker=3
```

`start`

�Ұʤ@�ӪA�Ȥw�g�s�b���e��.

`stop`

����@�Ӥw�g�B�檺�e���A�����R�����C�q�L `fig start` �i�H�A���Ұʳo�Ǯe���C

`up`

�c�ءA�]���s�^�ЫءA�ҰʡA�챵�@�ӪA�Ȫ��e���C

�챵���A�ȳ��N�|�ҰʡA���D�L�̤w�g�B��C

�q�{���p�A `fig up` �N�|�E�X�C�Ӯe������X�A�ӥB�p�G�e���w�g�s�b�A�Ҧ��e���N�|����C�p�G�A�B�� `fig up -d` �A�N�|�b��x�ҰʨùB��Ҧ����e���C

�q�{���p�A�p�G�o�ӪA�Ȫ��e���w�g�s�b�A `fig up` �N�|����í��s�ЫإL�̡]�O���ϥ�volumes-from���������^�A�H�O�� `fig.yml` ���ק�ͮġC�p�G�A���Q�e���Q����í��s�ЫءA�i�H�ϥ� `fig up --no-recreate` �C�p�G�ݭn���ܡA�o�˱N�|�Ұʤw�g����e���C

###�����ܶq

�����ܶq�i�H�ΨӰt�mFig���欰�C

�ܶq�HDOCKER_�}�Y�A���̩M�ΨӰt�mDocker�R�O��Ȥ�ݪ��ϥΤ@�ˡC�p�G�A�b�ϥ� boot2docker , `$(boot2docker shellinit)` �N�|�]�m���̬����T���ȡC

`FIG_PROJECT_NAME`

�]�m�q�LFig�Ұʪ��C�@�Ӯe���e�K�[�����ئW��.�q�{�O��e�u�@�ؿ����W�r�C

`FIG_FILE`

�]�m�n�ϥΪ� `fig.yml` �����|�C�q�{���|�O��e�u�@�ؿ��C

`DOCKER_HOST`

�]�mdocker�i�{��URL�C�q�{docker client�ϥ� `unix:///var/run/docker.sock` �C

`DOCKER_TLS_VERIFY`

�p�G�]�m�����Ū��r�šA���\�M�i�{�i�� TLS �q�H�C

`DOCKER_CERT_PATH`

�t�m `ca.pem` �����|�A `cert.pem` �M `key.pem` ���ΨӶi��TLS����.�q�{���|�O `~/.docker` �C
