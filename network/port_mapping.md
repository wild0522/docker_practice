## 德□□e餤婥H邦碗臺□3原A□□□]婥H□□o臀□□i刳q錠`-P` 鋼`-p` □□□□□f□g□
□□Π-P □OADocker 綴HM@□`49000~49900` □□f謤□隼□□□□f□
菥Π`docker ps` 婥H□□廦a齉窺 49155 □M□□ 5000 □f□鯛□□□□49155 □f癥i□□e□eb 3厴□□□□
```
$ sudo docker run -d -P training/webapp python app.py
$ sudo docker ps -l
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse
```
拳□□婥H□L `docker logs` 鑰O蓬d□□□HC
```
$ sudo docker logs -f nostalgic_morse
* Running on http://0.0.0.0:5000/
10.0.2.2 - - [23/May/2014 20:16:31] "GET / HTTP/1.1" 200 -
10.0.2.2 - - [23/May/2014 20:16:31] "GET /favicon.ico HTTP/1.1" 404 -
```

-p□p□□□i別□□M□□A胥B□b□□□□f□u婥H□w□□eC□□□ `ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort`□
### □g欽□□a罊菥Π`hostPort:containerPort` □□□000 □f□g謮e□000 □f□i到妦
```
$ sudo docker run -d -p 5000:5000 training/webapp python app.py
```
湮□q□|□w廦a欽□□W□□□罡C

### □g謫□檜}□□□f
婥H菥Π`ip:hostPort:containerPort` □鷬M□Τ@□S鷦a罡A□p localhost 檜} 127.0.0.1
```
$ sudo docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
```
### □g謫□檜}□□□f
菥Π`ip::containerPort` □w localhost □□□f謮e□000 □f□□齉縕曰□0t□□□f□```
$ sudo docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```
Y婥H菥Πudp □O蓫□ udp □f
```
$ sudo docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
```
### □□M□f□m
菥Π`docker port` 蓬d□□□g□□f□m□]婥H□□□鶯□罊```
$ docker port nostalgic_morse 5000
127.0.0.1:49155.
```
□N□* 馳曤v□□□詍 ip 檜}□□Π`docker inspect` 婥H2躩□□□污ADocker Y婥H□@□i□□□□m□^
* -p □O婥H樦訏廚□j鷦h□□f

擭p
```
$ sudo docker run -d -p 5000:5000  -p 3000:80 training/webapp python app.py
```
