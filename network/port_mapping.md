## �~���X�ݮe��
�e�����i�H�B��@�Ǻ������ΡA�n���~���]�i�H�X�ݳo�����ΡA�i�H�q�L `-P` �� `-p` �Ѽƨӫ��w�ݤf�M�g�C

��ϥ� -P �аO�ɡADocker �|�H���M�g�@�� `49000~49900` ���ݤf�줺���e���}�񪺺����ݤf�C

�ϥ� `docker ps` �i�H�ݨ�A���a�D���� 49155 �Q�M�g��F�e���� 5000 �ݤf�C���ɳX�ݥ����� 49155 �ݤf�Y�i�X�ݮe���� web ���δ��Ѫ��ɭ��C
```
$ sudo docker run -d -P training/webapp python app.py
$ sudo docker ps -l
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse
```
�P�˪��A�i�H�q�L `docker logs` �R�O�Ӭd�����Ϊ��H���C
```
$ sudo docker logs -f nostalgic_morse
* Running on http://0.0.0.0:5000/
10.0.2.2 - - [23/May/2014 20:16:31] "GET / HTTP/1.1" 200 -
10.0.2.2 - - [23/May/2014 20:16:31] "GET /favicon.ico HTTP/1.1" 404 -
```

-p�]�p�g���^�h�i�H���w�n�M�g���ݤf�A�åB�A�b�@�ӫ��w�ݤf�W�u�i�H�j�w�@�Ӯe���C������榡�� `ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort`�C

### �M�g�Ҧ����f�a�}
�ϥ� `hostPort:containerPort` �榡���a�� 5000 �ݤf�M�g��e���� 5000 �ݤf�A�i�H����
```
$ sudo docker run -d -p 5000:5000 training/webapp python app.py
```
�����q�{�|�j�w���a�Ҧ����f�W���Ҧ��a�}�C

### �M�g����w�a�}�����w�ݤf
�i�H�ϥ� `ip:hostPort:containerPort` �榡���w�M�g�ϥΤ@�ӯS�w�a�}�A��p localhost �a�} 127.0.0.1
```
$ sudo docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
```
### �M�g����w�a�}�����N�ݤf
�ϥ� `ip::containerPort` �j�w localhost �����N�ݤf��e���� 5000 �ݤf�A���a�D���|�۰ʤ��t�@�Ӻݤf�C
```
$ sudo docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```
�٥i�H�ϥ� udp �аO�ӫ��w udp �ݤf
```
$ sudo docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
```
### �d�ݬM�g�ݤf�t�m
�ϥ� `docker port` �Ӭd�ݷ�e�M�g���ݤf�t�m�A�]�i�H�d�ݨ�j�w���a�}
```
$ docker port nostalgic_morse 5000
127.0.0.1:49155.
```
�`�N�G
* �e�����ۤv�����������M ip �a�}�]�ϥ� `docker inspect` �i�H����Ҧ����ܶq�ADocker �٥i�H���@�ӥi�ܪ������t�m�C�^
* -p �аO�i�H�h���ϥΨӸj�w�h�Ӻݤf

�Ҧp
```
$ sudo docker run -d -p 5000:5000  -p 3000:80 training/webapp python app.py
```
