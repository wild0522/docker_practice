##匯出和 匯入容器

###匯出容器
如果要匯出本地某個容器，可以使用 `docker export` 指令。
```
$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:14.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ sudo docker export 7691a814370e > ubuntu.tar
```
這樣將匯出容器快照到本地檔案。

### 匯入容器快照
可以使用 `docker import` 從容器快照檔案中再 匯入為映像檔，例如
```
$ cat ubuntu.tar | sudo docker import - test/ubuntu:v1.0
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
```
此外，也可以透過指定 URL 或是某個目錄來 匯入，例如
```
$sudo docker import http://example.com/exampleimage.tgz example/imagerepo
```

*註：使用者既可以使用 `docker load` 來 匯入映像檔儲存檔案到本地映像檔庫，也可以使用 `docker import` 來 匯入一個容器快照到本地映像檔庫。這兩者的區別在於容器快照檔案將丟棄所有的歷史記錄和元資料資訊（即僅儲存容器當時的快照狀態），而映像檔儲存檔案將儲存完整記錄，體積也要大。此外，從容器快照檔案 匯入時可以重新指定標籤等元資料資訊。


