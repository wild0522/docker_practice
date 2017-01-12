## 安裝與卸載

Compose 目前支援 Linux 和 Mac OS 平台，兩者的安裝程序大同小異。

安裝 Compose 之前，要先安裝 Docker（需要 Docker Engine 1.7.1+），請參考第一部分中章節，在此不再贅述。

Compose 可以透過 Python 的 pip 工具進行安裝，可以直接下載編譯好的二進位檔案使用，甚至直接執行在 Docker 容器中。

前兩種方式是傳統方式，適合本地環境下安裝使用；最後一種方式則不破壞系統環境，更適合雲計算場景。

### PIP 安裝
這種方式是將 Compose 當作一個 Python 應用來從 pip 源中安裝。

執行安裝指令：

```sh
$ sudo pip install -U docker-compose
```

可以看到類似如下輸出，說明安裝成功。
```sh
Collecting docker-compose
  Downloading docker-compose-1.8.0.tar.gz (149kB): 149kB downloaded
...
Successfully installed docker-compose cached-property requests texttable websocket-client docker-py dockerpty six enum34 backports.ssl-match-hostname ipaddress
```

安裝成功後，可以 檢視 `docker-compose` 指令的使用方式。
```sh
$ docker-compose -h
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE           Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME   Specify an alternate project name (default: directory name)
  --x-networking            (EXPERIMENTAL) Use new Docker networking functionality.
                            Requires Docker 1.9 or later.
  --x-network-driver DRIVER (EXPERIMENTAL) Specify a network driver (default: "bridge").
                            Requires Docker 1.9 or later.
  --verbose                 Show more output
  -v, --version             Print version and exit

Commands:
  build              Build or rebuild services
  help               Get help on a command
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pulls service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  unpause            Unpause services
  up                 Create and start containers
  migrate-to-labels  Recreate containers to add labels
  version            Show the Docker-Compose version information
```

之後，可以加入 bash 補全指令。

```sh
$ curl -L https://raw.githubusercontent.com/docker/compose/1.8.0/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

### 二進位包
官方定義編譯好二進位包，供大家使用。這些發佈的二進位包可以在 [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases) 頁面找到。

這些二進位檔案，下載後直接放到執行路徑下，並加入執行權限即可。

例如，在 Linux 平台上。

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
$ sudo chmod a+x /usr/local/bin/docker-compose
```

可以使用 `docker-compose version` 指令來 檢視版本資訊，以測試是否安裝成功。

```sh
$ docker-compose version
docker-compose version 1.8.0, build 94f7016
docker-py version: 1.9.0
CPython version: 2.7.6
OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
```

### 容器中執行

Compose 既然是一個 Python 應用，自然也可以直接用容器來執行它。

```sh
$ curl -L https://github.com/docker/compose/releases/download/1.8.0/run.sh > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
```

實際上， 檢視下載的 `run.sh` 指令碼內容，如下

```sh
set -e

VERSION="1.8.0"
IMAGE="docker/compose:$VERSION"


# Setup options for connecting to docker host
if [ -z "$DOCKER_HOST" ]; then
    DOCKER_HOST="/var/run/docker.sock"
fi
if [ -S "$DOCKER_HOST" ]; then
    DOCKER_ADDR="-v $DOCKER_HOST:$DOCKER_HOST -e DOCKER_HOST"
else
    DOCKER_ADDR="-e DOCKER_HOST -e DOCKER_TLS_VERIFY -e DOCKER_CERT_PATH"
fi


# Setup volume mounts for compose config and context
if [ "$(pwd)" != '/' ]; then
    VOLUMES="-v $(pwd):$(pwd)"
fi
if [ -n "$COMPOSE_FILE" ]; then
    compose_dir=$(dirname $COMPOSE_FILE)
fi
# TODO: also check --file argument
if [ -n "$compose_dir" ]; then
    VOLUMES="$VOLUMES -v $compose_dir:$compose_dir"
fi
if [ -n "$HOME" ]; then
    VOLUMES="$VOLUMES -v $HOME:$HOME -v $HOME:/root" # mount $HOME in /root to share docker.config
fi

# Only allocate tty if we detect one
if [ -t 1 ]; then
    DOCKER_RUN_OPTIONS="-t"
fi
if [ -t 0 ]; then
    DOCKER_RUN_OPTIONS="$DOCKER_RUN_OPTIONS -i"
fi

exec docker run --rm $DOCKER_RUN_OPTIONS $DOCKER_ADDR $COMPOSE_OPTIONS $VOLUMES -w "$(pwd)" $IMAGE "$@"
```

可以看到，它其實是下載了 `docker/compose` 映像檔並執行。

### 卸載
如果是二進位包方式安裝的，刪除二進位檔案即可。

```sh
$ sudo rm /usr/local/bin/docker-compose
```

如果是透過 python pip 工具安裝的，則可以執行如下指令刪除。

```sh
$ sudo pip uninstall docker-compose
```