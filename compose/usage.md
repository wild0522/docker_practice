## �ϥ�

### �N�y
�������дX�ӳN�y�C

* �A�ȡ]service�^�G�@�����ήe���A��ڤW�i�H�B��h�ӬۦP�蹳����ҡC
* ����(project)�G�Ѥ@�����p�����ήe���զ����@�ӧ���~�ȳ椸�C

�i���A�@�Ӷ��إi�H�Ѧh�ӪA�ȡ]�e���^���p�Ӧ��ACompose ���V���ضi��޲z�C

### ����
�U���A�ڭ̳Ыؤ@�Ӹg�媺 Web ���ءG�@�� [Haproxy](www.haproxy.org)�A�����T�� Web �e���C

�Ыؤ@�� `compose-haproxy-web` �ؿ��A�@�����ؤu�@�ؿ��A�æb�䤤���O�Ыب�Ӥl�ؿ��G`haproxy` �M `web`�C

### Web �l�ؿ�

�o�̥� Python �{�ǨӴ��Ѥ@��²�檺 HTTP �A�ȡA���L�X�X�ݪ̪� IP �M ��ڪ����a IP�C

#### index.py

�s�g�@�� `index.py` �@���A�Ⱦ����A�N�X��
```sh
#!/usr/bin/python
#authors: yeasy.github.com
#date: 2013-07-05

import sys
import BaseHTTPServer
from SimpleHTTPServer import SimpleHTTPRequestHandler
import socket
import fcntl
import struct
import pickle
from datetime import datetime
from collections import OrderedDict

class HandlerClass(SimpleHTTPRequestHandler):
    def get_ip_address(self,ifname):
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        return socket.inet_ntoa(fcntl.ioctl(
            s.fileno(),
            0x8915,  # SIOCGIFADDR
            struct.pack('256s', ifname[:15])
        )[20:24])
    def log_message(self, format, *args):
        if len(args) < 3 or "200" not in args[1]:
            return
        try:
            request = pickle.load(open("pickle_data.txt","r"))
        except:
            request=OrderedDict()
        time_now = datetime.now()
        ts = time_now.strftime('%Y-%m-%d %H:%M:%S')
        server = self.get_ip_address('eth0')
        host=self.address_string()
        addr_pair = (host,server)
        if addr_pair not in request:
            request[addr_pair]=[1,ts]
        else:
            num = request[addr_pair][0]+1
            del request[addr_pair]
            request[addr_pair]=[num,ts]
        file=open("index.html", "w")
        file.write("<!DOCTYPE html> <html> <body><center><h1><font color=\"blue\" face=\"Georgia, Arial\" size=8><em>HA</em></font> Webpage Visit Results</h1></center>");
        for pair in request:
            if pair[0] == host:
                guest = "LOCAL: "+pair[0]
            else:
                guest = pair[0]
            if (time_now-datetime.strptime(request[pair][1],'%Y-%m-%d %H:%M:%S')).seconds < 3:
                file.write("<p style=\"font-size:150%\" >#"+ str(request[pair][1]) +": <font color=\"red\">"+str(request[pair][0])+ "</font> requests " + "from &lt<font color=\"blue\">"+guest+"</font>&gt to WebServer &lt<font color=\"blue\">"+pair[1]+"</font>&gt</p>")
            else:
                file.write("<p style=\"font-size:150%\" >#"+ str(request[pair][1]) +": <font color=\"maroon\">"+str(request[pair][0])+ "</font> requests " + "from &lt<font color=\"navy\">"+guest+"</font>&gt to WebServer &lt<font color=\"navy\">"+pair[1]+"</font>&gt</p>")
        file.write("</body> </html>");
        file.close()
        pickle.dump(request,open("pickle_data.txt","w"))

if __name__ == '__main__':
    try:
        ServerClass  = BaseHTTPServer.HTTPServer
        Protocol     = "HTTP/1.0"
        addr = len(sys.argv) < 2 and "0.0.0.0" or sys.argv[1]
        port = len(sys.argv) < 3 and 80 or int(sys.argv[2])
        HandlerClass.protocol_version = Protocol
        httpd = ServerClass((addr, port), HandlerClass)
        sa = httpd.socket.getsockname()
        print "Serving HTTP on", sa[0], "port", sa[1], "..."
        httpd.serve_forever()
    except:
        exit()
```

#### index.html
�ͦ��@���{�ɪ� `index.html` ���A�䤺�e�|�Q index.py ��s�C
```sh
$ touch index.html
```

#### Dockerfile
�ͦ��@�� Dockerfile�A���e��
```sh
FROM python:2.7
WORKDIR /code
ADD . /code
EXPOSE 80
CMD python index.py
```

### haproxy �ؿ�
�b�䤤�ͦ��@�� `haproxy.cfg` ���A���e��
```sh
global
  log 127.0.0.1 local0
  log 127.0.0.1 local1 notice

defaults
  log global
  mode http
  option httplog
  option dontlognull
  timeout connect 5000ms
  timeout client 50000ms
  timeout server 50000ms

listen stats :70
    stats enable
    stats uri /

frontend balancer
    bind 0.0.0.0:80
    mode http
    default_backend web_backends

backend web_backends
    mode http
    option forwardfor
    balance roundrobin
    server weba weba:80 check
    server webb webb:80 check
    server webc webc:80 check
    option httpchk GET /
    http-check expect status 200
```
### docker-compose.yml
�s�g docker-compose.yml ���A�o�ӬO Compose �ϥΪ��D�ҪO���C���e�Q��²��A���w 3 �� web �e���A�H�� 1 �� haproxy �e���C

```sh
weba:
    build: ./web
    expose:
        - 80

webb:
    build: ./web
    expose:
        - 80

webc:
    build: ./web
    expose:
        - 80

haproxy:
    image: haproxy:latest
    volumes:
        - haproxy:/haproxy-override
        - haproxy/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg:ro
    links:
        - weba
        - webb
        - webc
    ports:
        - "80:80"
        - "70:70"
    expose:
        - "80"
        - "70"
```

### �B�� compose ����
�{�b compose-haproxy-web �ؿ������U�����ˤl�C
```sh
compose-haproxy-web
�u�w�w docker-compose.yml
�u�w�w haproxy
�x   �|�w�w haproxy.cfg
�|�w�w web
    �u�w�w Dockerfile
    �u�w�w index.html
    �|�w�w index.py
```
�b�ӥؿ��U���� `docker-compose up` �R�O�A�|��X��X�Ҧ��e������X�C
```
$sudo docker-compose up
Recreating composehaproxyweb_webb_1...
Recreating composehaproxyweb_webc_1...
Recreating composehaproxyweb_weba_1...
Recreating composehaproxyweb_haproxy_1...
Attaching to composehaproxyweb_webb_1, composehaproxyweb_webc_1, composehaproxyweb_weba_1, composehaproxyweb_haproxy_1
```

���ɳX�ݥ��a�� 80 �ݤf�A�|�g�L haproxy �۰���o���ݪ��Y�� web �e���W�A��s�����A�i�H�[���X�ݪ��e���a�}���ܤơC

�X�ݥ��a 70 �ݤf�A�i�H�d�ݨ� haproxy ���έp�H���C

��M�A�٥i�H�ϥ� consul�Betcd ����{�A�ȵo�{�A�o�˴N�i�H�קK��ʫ��w��ݪ� web �e���F�A���F���C
