##�ϥ� Django �J�� Fig

�ڭ̲{�b�N�ϥ� Fig �t�m�ùB��@�� Django/PostgreSQL ���ΡC�b�����e�A���T�O Fig �w�g [�w��](install.md)�C

�b�@���u�@�}�l�e�A�ݭn���]�m�n�T�ӥ��n�����C  
�Ĥ@�B�A�]�����αN�n�B��b�@�Ӻ����Ҧ����Ҩ̿઺ Docker �e���̭��A����ڭ̥i�H�q�L�s�� `Dockerfile` ���ӫ��w Docker �e���n�w�ˤ��e�C���e�p�U�G 

```
FROM python:2.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```
�H�W���e���w���αN�ϥΦw�ˤF Python �H�Υ��n�̿�]���蹳�C��h����p��s�g Dockerfile ��󪺫H���i�H�d�� [�蹳�Ы�](../image/create.md#�Q�� Dockerfile �ӳЫ��蹳) �M [Dockerfile �ϥ�](../dockerfile/README.md)�C

�ĤG�B�A�b `requirements.txt` ���̭��g���ݭn�w�˪�����̿�]�W �C

```
Django
psycopg2
```

�N�O�o��²��C  
�ĤT�B�A`fig.yml` ���N��Ҧ����F�����p�_�ӡC���y�z�F���Ϊ��c���]�@�� web �A�ȩM�@�Ӽƾڮw�^�B�ϥΪ� Docker �蹳�B�蹳�������s���B������e�������A�H�ΪA�ȶ}�񪺺ݤf�C 

```
db:
  image: postgres
web:
  build: .
  command: python manage.py runserver 0.0.0.0:8000
  volumes:
    - .:/code
  ports:
    - "8000:8000"
  links:
    - db
```
�d�� [`fig.yml` ���`](yml_ref.md) �A�ѧ�h�ԲӪ��u�@����C

�{�b�ڭ̴N�i�H�ϥ� `fig run` �R�O�Ұʤ@�� Django ���ΤF�C

```
$ fig run web django-admin.py startproject figexample .
```
Fig �|���ϥ� `Dockerfile` �� web �A�ȳЫؤ@���蹳�A���ۨϥγo���蹳�b�e���̹B�� `django-admin.py startproject figexample . ` ���O�C

�o�N�b��e�ؿ��ͦ��@�� Django ���ΡC

```
$ ls
Dockerfile       fig.yml          figexample       manage.py       requirements.txt
```
�����A�ڭ̭n�����γ]�m�n�ƾڮw���s���H���C�ΥH�U���e���� `figexample/settings.py` ��� `DATABASES = ...` �w�q���`�I���e�C

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```
�o�ǫH���O�b [postgres](https://registry.hub.docker.com/_/postgres/) Docker �蹳�T�w�]�m�n���C  
�M��A�B�� `fig up` �G
	
```
Recreating myapp_db_1...
Recreating myapp_web_1...
Attaching to myapp_db_1, myapp_web_1
myapp_db_1 |
myapp_db_1 | PostgreSQL stand-alone backend 9.1.11
myapp_db_1 | 2014-01-27 12:17:03 UTC LOG:  database system is ready to accept connections
myapp_db_1 | 2014-01-27 12:17:03 UTC LOG:  autovacuum launcher started
myapp_web_1 | Validating models...
myapp_web_1 |
myapp_web_1 | 0 errors found
myapp_web_1 | January 27, 2014 - 12:12:40
myapp_web_1 | Django version 1.6.1, using settings 'figexample.settings'
myapp_web_1 | Starting development server at http://0.0.0.0:8000/
myapp_web_1 | Quit the server with CONTROL-C.
```
�o�� web ���Τw�g�}�l�b�A�� docker �u�@�i�{�̺�ť�� 5000 �ݤf�F�]�p�G�A���ϥ� boot2docker �A���� `boot2docker ip` �A�N�|�ݨ쥦���a�}�^�C

�A�٥i�H�b Docker �W�B��䥦���޲z�R�O�A�Ҧp���P�B�ƾڮw���c�o�بơA�b�B�槹 `fig up` ��A�b�t�~�@�Ӳ׺ݹB��H�U�R�O�Y�i�G

```
$ fig run web python manage.py syncdb
```