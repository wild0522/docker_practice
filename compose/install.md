## �w��

�w�� Compose ���e�A�n���w�� Docker�A�b�����A�حz�C

### PIP �w��
�o�ؤ覡�̬����ˡC

����R�O�C
```sh
$ sudo pip install -U docker-compose
```

�w�˦��\��A�i�H�d�� `docker-compose` �R�O���Ϊk�C
```sh
$ docker-compose -h
Fast, isolated development environments using Docker.

Usage:
  docker-compose [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  --verbose                 Show more output
  --version                 Print version and exit
  -f, --file FILE           Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME   Specify an alternate project name (default: directory name)

Commands:
  build     Build or rebuild services
  help      Get help on a command
  kill      Kill containers
  logs      View output from containers
  port      Print the public port for a port binding
  ps        List containers
  pull      Pulls service images
  rm        Remove stopped containers
  run       Run a one-off command
  scale     Set number of containers for a service
  start     Start services
  stop      Stop services
  restart   Restart services
  up        Create and start containers
```

����A�i�H�K�[ bash �ɥ��R�O�C
```sh
$ curl -L https://raw.githubusercontent.com/docker/compose/1.2.0/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose

```

### �G�i��]
�o�G���G�i��]�i�H�b [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases) ���C

�U���᪽����������|�Y�i�C

�Ҧp�A�b�`���� Linux ���x�W�C

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
$ sudo chmod a+x /usr/local/bin/docker-compose
```


