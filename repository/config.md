## �ܮw�t�m���
Docker �� Registry �Q�ΰt�m��󴣨ѤF�@�ǭܮw���ҪO�]flavor�^�A�Τ�i�H�����ϥΥ��̨Ӷi��}�o�ΥͲ����p�C

### �ҪO
�b `config_sample.yml` ��󤤡A�i�H�ݨ�@�ǲ{�����ҪO�q�G
* `common`�G��¦�t�m
* `local`�G�s�x�ƾڨ쥻�a���t��
* `s3`�G�s�x�ƾڨ� AWS S3 ��
* `dev`�G�ϥ� `local` �ҪO���򥻰t�m
* `test`�G�椸���ըϥ�
* `prod`�G�Ͳ����Ұt�m�]�򥻤W��s3�t�m�����^
* `gcs`�G�s�x�ƾڨ� Google �����s�x
* `swift`�G�s�x�ƾڨ� OpenStack Swift �A��
* `glance`�G�s�x�ƾڨ� OpenStack Glance �A�ȡA���a���t�ά����
* `glance-swift`�G�s�x�ƾڨ� OpenStack Glance �A�ȡASwift �����
* `elliptics`�G�s�x�ƾڨ� Elliptics key/value �s�x

�Τ�]�i�H�K�[�۩w�q���Ҫ��q�C

�q�{���p�U�ϥΪ��ҪO�O `dev`�A�n�ϥάY�ӼҪO�@���q�{�ȡA�i�H�K�[ `SETTINGS_FLAVOR` �������ܶq���A�Ҧp
```
export SETTINGS_FLAVOR=dev
```

�t�~�A�t�m��󤤤���q�����ܶq���[���ȡA�y�k�榡�� `_env:VARIABLENAME[:DEFAULT]`�C

### �ܨҰt�m
```
common:
    loglevel: info
    search_backend: "_env:SEARCH_BACKEND:"
    sqlalchemy_index_database:
        "_env:SQLALCHEMY_INDEX_DATABASE:sqlite:////tmp/docker-registry.db"

prod:
    loglevel: warn
    storage: s3
    s3_access_key: _env:AWS_S3_ACCESS_KEY
    s3_secret_key: _env:AWS_S3_SECRET_KEY
    s3_bucket: _env:AWS_S3_BUCKET
    boto_bucket: _env:AWS_S3_BUCKET
    storage_path: /srv/docker
    smtp_host: localhost
    from_addr: docker@myself.com
    to_addr: my@myself.com

dev:
    loglevel: debug
    storage: local
    storage_path: /home/myself/docker

test:
    storage: local
    storage_path: /tmp/tmpdockertmp
```

### �ﶵ
