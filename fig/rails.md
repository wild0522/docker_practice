##�ϥ� Rail �J�� Fig

�ڭ̲{�b�N�ϥ� Fig �t�m�ùB��@�� Rails/PostgreSQL ���ΡC�b�}�l���e�A���T�O Fig �w�g [�w��](install.md)�C


�b�@���u�@�}�l�e�A�ݭn���]�m�n�T�ӥ��n�����C  
�����A�]�����αN�n�B��b�@�Ӻ����Ҧ����Ҩ̿઺ Docker �e���̭��A����ڭ̥i�H�q�L�s�� `Dockerfile` ���ӫ��w Docker �e���n�w�ˤ��e�C���e�p�U�G 

```
FROM ruby
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev
RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
RUN bundle install
ADD . /myapp
```
�H�W���e���w���αN�ϥΦw�ˤF Ruby�BBundler �H�Ψ�̿���蹳�C��h����p��s�g Dockerfile ��󪺫H���i�H�d�� [�蹳�Ы�](../image/create.md#�Q�� Dockerfile �ӳЫ��蹳) �M [Dockerfile �ϥ�](../dockerfile/README.md)�C
�U�@�B�A�ڭ̻ݭn�@�Ӥ޾ɥ[�� Rails ����� `Gemfile` �C ���@�|�०�ٷ|�Q `rails new` �R�O�л\���g�C

```
source 'https://rubygems.org'
gem 'rails', '4.0.2'
```
�̫�A`fig.yml` ���~�O�̯��_���a��C `fig.yml` ���N��Ҧ����F�����p�_�ӡC���y�z�F���Ϊ��c���]�@�� web �A�ȩM�@�Ӽƾڮw�^�B�C���蹳���ӷ��]�ƾڮw�B��b�ϥιw�w�q�� PostgreSQL �蹳�Aweb ���ΰ��N�q���a�ؿ��Ыء^�B�蹳�������s���A�H�ΪA�ȶ}�񪺺ݤf�C

```
db:
  image: postgres
  ports:
    - "5432"
web:
  build: .
  command: bundle exec rackup -p 3000
  volumes:
    - .:/myapp
  ports:
    - "3000:3000"
  links:
    - db
```
�Ҧ����N����A�ڭ̴N�i�H�q�L�ϥ� `fig run` �R�O�ͦ����Ϊ����[�F�C 
 
```
$ fig run web rails new . --force --database=postgresql --skip-bundle
```
Fig �|���ϥ� `Dockerfile` �� web �A�ȳЫؤ@���蹳�A���ۨϥγo���蹳�b�e���̹B�� `rails new ` �M�����᪺�R�O�C�@���o�өR�O�B�槹��A���ӴN�i�H�ݤ@�ӹ�s�����Τw�g�ͦ��F�C

```
$ ls
Dockerfile   app          fig.yml      tmp
Gemfile      bin          lib          vendor
Gemfile.lock config       log
README.rdoc  config.ru    public
Rakefile     db           test
```
�b�s�� `Gemfile` ���h���[�� `therubyracer` ���檺�����A�o�˧ڭ̫K�i�H�ϥ� Javascript �B�����ҡG

```
gem 'therubyracer', platforms: :ruby
```
�{�b�ڭ̤w�g���@�ӷs�� `Gemfile` ���A�ݭn�A���s�Ы��蹳�C�]�o�ӷ|�B�J�|���� Dockerfile ��󥻨��A�ȶȻݭn���ؤ@���^�C

```
$ fig build
```
���β{�b�N�i�H�ҰʤF�A���t�m�٥������CRails �q�{Ū�����ƾڮw�ؼЬO `localhost` �A�ڭ̻ݭn��ʫ��w�e���� `db` �C�P�˪��A�ٻݭn��Τ�W�ק令�M postgres �蹳�w�w���@�P�C
���}�̷s�ͦ��� `database.yml` ���C�ΥH�U���e�����G

```
development: &default
  adapter: postgresql
  encoding: unicode
  database: postgres
  pool: 5
  username: postgres
  password:
  host: db

test:
  <<: *default
  database: myapp_test
```
�{�b�N�i�H�Ұ����ΤF�C

```
$ fig up
```
�p�G�@�����`�A�A���ӥi�H�ݨ� PostgreSQL ����X�A�X���i�H�ݨ�o�˪����ƫH���G

```
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick 1.3.1
myapp_web_1 | [2014-01-17 17:16:29] INFO  ruby 2.0.0 (2013-11-22) [x86_64-linux-gnu]
myapp_web_1 | [2014-01-17 17:16:29] INFO  WEBrick::HTTPServer#start: pid=1 port=3000
```
�̫�A �ڭ̻ݭn�����O�Ыؼƾڮw�A���}�t�@�Ӳ׺ݡA�B��G

```
$ fig run web rake db:create
```
�o�� web ���Τw�g�}�l�b�A�� docker �u�@�i�{�̭���ť�� 3000 �ݤf�F�]�p�G�A���ϥ� boot2docker �A���� `boot2docker ip` �A�N�|�ݨ쥦���a�}�^�C

![](../_images/fig-rails-screenshot.png)