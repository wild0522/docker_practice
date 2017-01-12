##後台(background)執行

更多的時候，需要讓 Docker在後台執行而不是直接把執行指令的結果輸出在目前宿主電腦下。此時，可以透過加入 `-d` 參數來實作。

下面舉兩個例子來說明一下。

如果不使用 `-d` 參數執行容器。
```
$ sudo docker run ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
hello world
hello world
hello world
hello world
```
容器會把輸出的結果(STDOUT)列印到宿主電腦上面

如果使用了 `-d` 參數執行容器。
```
$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
77b2dc01fe0f3f1265df143181e7b9af5e05279a884f4776ee75350ea9d8017a
```
此時容器會在後台執行並不會把輸出的結果(STDOUT)列印到宿主電腦上面(輸出結果可以用docker logs  檢視)。

**註：** 容器是否會長久執行，是和docker run指定的指令有關，和 `-d` 參數無關。

使用 `-d` 參數啟動後會傳回一個唯一的 id，也可以透過 `docker ps` 指令來 檢視容器資訊。
```
$ sudo docker ps
CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
77b2dc01fe0f  ubuntu:14.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        agitated_wright
```
要取得容器的輸出資訊，可以透過 `docker logs` 指令。
```
$ sudo docker logs [container ID or NAMES]
hello world
hello world
hello world
. . .
```
