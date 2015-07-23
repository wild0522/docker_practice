## �ƾڨ�
�ƾڨ��O�@�ӥi�Ѥ@�өΦh�Ӯe���ϥΪ��S��ؿ��A��¶�L UFS�A�i�H���ѫܦh���Ϊ��S�ʡG
* �ƾڨ��i�H�b�e�������@�ɩM����
* ��ƾڨ����ק�|�߰��ͮ�
* ��ƾڨ�����s�A���|�v�T�蹳
* �ƾڨ��q�{�|�@���s�b�A�Y�Ϯe���Q�R��


*�`�N�G�ƾڨ����ϥΡA������ Linux �U��ؿ��Τ��i�� mount�A�蹳�����Q���w�������I���ؿ��������|���ñ��A����ܬݪ��O�������ƾڨ��C


### �Ыؤ@�Ӽƾڨ�
�b�� `docker run` �R�O���ɭԡA�ϥ� `-v` �аO�ӳЫؤ@�Ӽƾڨ��ñ�����e���̡C�b�@�� run ���h���ϥΥi�H�����h�Ӽƾڨ��C

�U���Ыؤ@�ӦW�� web ���e���A�å[���@�Ӽƾڱ���e���� `/webapp` �ؿ��C
```
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```
*�`�N�G�]�i�H�b Dockerfile ���ϥ� `VOLUME` �ӲK�[�@�өΪ̦h�ӷs������Ѹ��蹳�Ыت����N�e���C

### �R���ƾڨ�
�ƾڨ��O�Q�]�p�Ψӫ��[�Ƽƾڪ��A�����ͩR�g���W�ߩ�e���ADocker���|�b�e���Q�R����۰ʧR���ƾڨ��A�åB�]���s�b�U���^���o�˪�����ӳB�z�S������e���ޥΪ��ƾڨ��C�p�G�ݭn�b�R���e�����P�ɲ����ƾڨ��C�i�H�b�R���e�����ɭԨϥ� `docker rm -v` �o�өR�O�C�L�D���ƾڨ��i��|���ګܦh�Ŷ��A�n�M�z�|�ܳ·СCDocker�x�西�b�չϸѨM�o�Ӱ��D�A�����u�@���i�ץi�H�d�ݳo��[PR](https://github.com/docker/docker/pull/8484)

### �����@�ӥD���ؿ��@���ƾڨ�
�ϥ� `-v` �аO�]�i�H���w�����@�ӥ��a�D�����ؿ���e�����h�C
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```
�W�����R�O�[���D���� `/src/webapp` �ؿ���e���� `/opt/webapp`
�ؿ��C�o�ӥ\��b�i����ժ��ɭԤQ����K�A��p�Τ�i�H��m�@�ǵ{�Ǩ쥻�a�ؿ����A�Ӭd�ݮe���O�_���`�u�@�C���a�ؿ������|�����O������|�A�p�G�ؿ����s�b Docker �|�۰ʬ��A�Ыإ��C

*�`�N�GDockerfile ��������o�إΪk�A�o�O�]�� Dockerfile �O���F���өM���ɥΪ��C�M�ӡA���P�ާ@�t�Ϊ����|�榡���@�ˡA�ҥH�ثe�٤������C

Docker �����ƾڨ����q�{�v���OŪ�g�A�Τ�]�i�H�q�L `:ro` ���w���uŪ�C
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro
training/webapp python app.py
```
�[�F `:ro` ����A�N�������uŪ�F�C

### �d�ݼƾڨ�������H��

�b�D���̨ϥΥH�U�R�O�i�H�d�ݫ��w�e�����H��
```
$ docker inspect web
...
```

�b��X�����e�����䤤�M�ƾڨ������������A�i�H�ݨ�Ҧ����ƾڨ����O�Ыئb�D����`/var/lib/docker/volumes/`�U����
```
"Volumes": {
    "/webapp": "/var/lib/docker/volumes/fac362...80535"
},
"VolumesRW": {
    "/webapp": true
}
...
```

### �����@�ӥ��a�D�����@���ƾڨ�
`-v` �аO�]�i�H�q�D��������Ӥ���e����
```
$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
```
�o�˴N�i�H�O���b�e����J�L���R�O�F�C

*�`�N�G�p�G���������@�Ӥ��A�ܦh���s��u��A�]�A `vi` �Ϊ� `sed --in-place`�A�i��|�y����� inode �����ܡA�q Docker 1.1
.0�_�A�o�|�ɭP�����~�H���C�ҥH��²�檺��k�N����������󪺤��ؿ��C
