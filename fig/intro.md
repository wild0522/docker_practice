##�ֳt�f�ذ�� Docker ���j���}�o����

�ϥ� `Dockerfile` �����w�A���������ҡA������b���N�a��ƻs�ϥΡG

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
```

�b `fig.yml` ��󤤫��w���ΨϥΪ����P�A�ȡA�����̯���b�@�ӿW�ߪ����Ҥ��@�_�B��G

```
web:
  build: .
  command: python app.py
  links:
   - db
  ports:
   - "8000:8000"
db:
  image: postgres
```
**�`�N���ݭn�A�B�~�w�� Postgres �F�I*

���۰���R�O `fig up` �A�M�� Fig �N�|�ҰʨùB��A�����ΤF�C

![Docker](../_images/fig-example-large.gif)

Fig �i�Ϊ��R�O��:

* �ҰʡB����A�M���تA��
* �d�ݪA�Ȫ��B�檬�A
* �d�ݹB�椤���A�Ȫ���J��x
* ��A�ȵo�e�R�O

##�ֳt�W��
�ڭ̸յ����@�Ӱ򥻪� Python web ���ιB��b Fig �W�C�o�ӹ��簲�]�A�w�g���D�@�� Python ���ѡA�p�G�A�����x�A���M�������W���F��]�O�S�����D���C

�����A[�w�� Docker �M Fig](install.md)

���A�����سЫؤ@�ӥؿ�

```
$ mkdir figtest
$ cd figtest
```
�i�J�ؿ��A�Ы� `app.py`�A�o�O�@�ӯ���� Redis �W���@�ӭȦۼW��²�� web ���ΡA��� Flask �ج[�C

```
from flask import Flask
from redis import Redis
import os
app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    return 'Hello World! I have been seen %s times.' % redis.get('hits')

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```
�b `requirements.txt` ��󤤫��w���Ϊ� Python �̿�]�C

```
flask
redis
```
�U�@�B�ڭ̭n�Ыؤ@�ӥ]�t���ΩҦ��̿઺ Docker �蹳�A�o�̱N�ĭz���q�L `Dockerfile` ���ӳЫءC

```
FROM python:2.7
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
```
�H�W�����e�����i�D Docker �b�e���̭��w�� Python �A�N�X�����|�٦�Python �̿�]�C���� Dockerfile ����h�H���i�H�d�� [�蹳�Ы�](../image/create.md#�Q�� Dockerfile �ӳЫ��蹳) �M [Dockerfile �ϥ�](../dockerfile/README.md)

���ۧڭ̳q�L `fig.yml` �����w�@�t�C���A�ȡG

```
web:
  build: .
  command: python app.py
  ports:
   - "5000:5000"
  volumes:
   - .:/code
  links:
   - redis
redis:
  image: redis
  ```
�o�̫��w�F��ӪA�ȡG

* web �A�ȡA�q�L��e�ؿ��� `Dockerfile` �ЫءC�åB�����F�b�e���̭�����`python app.py ` �R�O �A��o�b�e���̶}�� 5000 �ݤf�쥻�a�D���� 5000 �ݤf�A�s�� Redis �A�ȡA�åB������e�ؿ���e���̭��A�o�˧ڭ̴N�i�H���έ����蹳�]�ઽ���ϥΥN�X�C
* redis �A�ȡA�ڭ̨ϥΤ����蹳 [redis](https://registry.hub.docker.com/_/redis/)�C
*
�{�b�p�G���� `fig up` �R�O �A���N�|�Ԩ� redis �蹳�A�ҰʩҦ����A�ȡC

```
$ fig up
Pulling image redis...
Building web...
Starting figtest_redis_1...
Starting figtest_web_1...
redis_1 | [8] 02 Jan 18:43:35.576 # Server started, Redis version 2.8.3
web_1   |  * Running on http://0.0.0.0:5000/
```
�o�� web ���Τw�g�}�l�b�A�� docker �u�@�i�{�̭���ť�� 5000 �ݤf�F�]�p�G�A���ϥ� boot2docker �A���� `boot2docker ip` �A�N�|�ݨ쥦���a�}�^�C

�p�G�A�Q�n�b��x�B��A���A�ȡA�i�H�b���� `fig up` �R�O���ɭԲK�[ `-d` �ѼơA�M��ϥ� `fig ps` �d�ݦ�����i�{�b�B��C

```
$ fig up -d
Starting figtest_redis_1...
Starting figtest_web_1...
$ fig ps
        Name                 Command            State       Ports
-------------------------------------------------------------------
figtest_redis_1   /usr/local/bin/run         Up
figtest_web_1     /bin/sh -c python app.py   Up      5000->5000/tcp
```

`fig run` ���O�i�H���A�V�A�ȵo�e�R�O�C�Ҧp�G�d�� web �A�ȥi�H����쪺�����ܶq:

```
$ fig run web env
```
�������U�R�O `fig --help` �d�ݨ䥦�i�Ϊ��ѼơC

���]�A�ϥΤF `fig up -d` �Ұ� Fig�A�i�H�q�L�H�U�R�O����A���A�ȡG

```
$ fig stop
```
�H�W���e�Φh�Τ֪����z�F�p��ϥ�Fig �C�q�L�d�ݤU�����ޥγ��`�i�H�A�Ѩ�����R�O�B�t�m�M�����ܶq����h�Ӹ`�C�p�G�A������Q�k�Ϋ�ĳ�A[�i�H�b GitHub �W���X](https://github.com/docker/fig)�C

