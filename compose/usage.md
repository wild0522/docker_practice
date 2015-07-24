## 使用

### 術語
首先介紹幾個術語。

* 服務（service）：一個應用容器，實際上可以運行多個相同鏡像的實例。
* 項目(project)：由一組關聯的應用容器組成的一個完整業務單元。

可見，一個項目可以由多個服務（容器）關聯而成，Compose 面向項目進行管理。

### 場景
下面，我們創建一個經典的 Web 項目：一個 [Haproxy](www.haproxy.org)，掛載三個 Web 容器。

創建一個 `compose-haproxy-web` 目錄，作為項目工作目錄，並在其中分別創建兩個子目錄：`haproxy` 和 `web`。

### Web 子目錄

這裡用 Python 程序來提供一個簡單的 HTTP 服務，打印出訪問者的 IP 和 實際的本地 IP。

#### index.py

編寫一個 `index.py` 作為服務器文件，代碼為
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
生成一個臨時的 `index.html` 文件，其內容會被 index.py 更新。
```sh
$ touch index.html
```

#### Dockerfile
生成一個 Dockerfile，內容為
```sh
FROM python:2.7
WORKDIR /code
ADD . /code
EXPOSE 80
CMD python index.py
```

### haproxy 目錄
在其中生成一個 `haproxy.cfg` 文件，內容為
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
編寫 docker-compose.yml 文件，這個是 Compose 使用的主模板文件。內容十分簡單，指定 3 個 web 容器，以及 1 個 haproxy 容器。

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

### 運行 compose 項目
現在 compose-haproxy-web 目錄長成下面的樣子。
```sh
compose-haproxy-web
├── docker-compose.yml
├── haproxy
│   └── haproxy.cfg
└── web
    ├── Dockerfile
    ├── index.html
    └── index.py
```
在該目錄下執行 `docker-compose up` 命令，會整合輸出所有容器的輸出。
```
$sudo docker-compose up
Recreating composehaproxyweb_webb_1...
Recreating composehaproxyweb_webc_1...
Recreating composehaproxyweb_weba_1...
Recreating composehaproxyweb_haproxy_1...
Attaching to composehaproxyweb_webb_1, composehaproxyweb_webc_1, composehaproxyweb_weba_1, composehaproxyweb_haproxy_1
```

此時訪問本地的 80 連接阜，會經過 haproxy 自動轉發到後端的某個 web 容器上，刷新頁面，可以觀察到訪問的容器地址的變化。

訪問本地 70 連接阜，可以查看到 haproxy 的統計信息。

當然，還可以使用 consul、etcd 等實現服務發現，這樣就可以避免手動指定後端的 web 容器了，更為靈活。
