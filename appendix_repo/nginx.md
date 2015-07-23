## [Nginx](https://registry.hub.docker.com/_/nginx/)

### �򥻫H��
[Nginx](https://en.wikipedia.org/wiki/Nginx) �O�}�������Ī� Web �A�Ⱦ���{�A��� HTTP�BHTTPS�BSMTP�BPOP3�BIMAP ����ĳ�C
�ӭܮw���ѤF Nginx 1.0 ~ 1.7 �U�Ӫ������蹳�C

### �ϥΤ�k
�U�����R�O�N�@���@���R�A�����A�Ⱦ��ҰʡC
```
$ sudo docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
```
�Τ�]�i�H���ϥγo�جM�g�覡�A�q�L�Q�� Dockerfile �Ӫ����N�R�A�������e����蹳���A���e��
```
FROM nginx
COPY static-html-directory /usr/share/nginx/html
```
����ͦ��s���蹳�A�ñҰʤ@�Ӯe���C
```
$ sudo docker build -t some-content-nginx .
$ sudo docker run --name some-nginx -d some-content-nginx
```
�}��ݤf�A�ìM�g�쥻�a�� `8080` �ݤf�C
```
sudo docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

Nginx���q�{�t�m�����|�� `/etc/nginx/nginx.conf`�A�i�H�q�L�M�g���ӨϥΥ��a���t�m���A�Ҧp
```
docker run --name some-nginx -v /some/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx
```
�ϥΰt�m���ɡA���F�b�e�������`�B��A�ݭn�O�� `daemon off;`�C

### Dockerfile
* [1 ~ 1.7 ����](https://github.com/nginxinc/docker-nginx/blob/3713a0157083eb4776e71f5a5aef4b2a5bc03ab1/Dockerfile)
