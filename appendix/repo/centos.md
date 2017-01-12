## [CentOS](https://hub.docker.com/_/centos/)

### 基本資訊
[CentOS](https://en.wikipedia.org/wiki/CentOS) 是流行的 Linux 發行版，其套裝軟體大多跟 RedHat 系列保持一致。

該倉庫位於 https://hub.docker.com/_/centos/，提供了 CentOS 從 5 ~ 7 各個版本的映像檔。

### 使用方法
 預設會啟動一個縮到最小的 CentOS 環境。

```sh
$ docker run --name some-centos -i -t centos bash
bash-4.2#
```

### Dockerfile

#### CentOS 5 版本
```
FROM scratch
MAINTAINER The CentOS Project <cloud-ops@centos.org>
ADD c5-docker.tar.xz /
LABEL name="CentOS Base Image" \
    vendor="CentOS" \
    license="GPLv2" \
    build-date="2016-03-31"

# Default command
CMD ["/bin/bash"]
```

#### CentOS 6 版本
```
FROM scratch
MAINTAINER https://github.com/CentOS/sig-cloud-instance-images
ADD centos-6-docker.tar.xz /

LABEL name="CentOS Base Image" \
    vendor="CentOS" \
    license="GPLv2" \
    build-date="20160729"

CMD ["/bin/bash"]
```

#### CentOS 7 版本
```
FROM scratch
MAINTAINER https://github.com/CentOS/sig-cloud-instance-images
ADD centos-7-docker.tar.xz /

LABEL name="CentOS Base Image" \
    vendor="CentOS" \
    license="GPLv2" \
    build-date="20160729"

CMD ["/bin/bash"]
```
