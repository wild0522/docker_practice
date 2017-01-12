## [Nginx](https://hub.docker.com/_/nginx/)

### 基本資訊
[Nginx](https://en.wikipedia.org/wiki/Nginx) 是開源的高效的 Web 伺服器實作，支援 HTTP、HTTPS、SMTP、POP3、IMAP 等協定。

該倉庫位於 https://hub.docker.com/_/nginx/，提供了 Nginx 1.0 ~ 1.11.x 各個版本的映像檔。

### 使用方法
下面的指令將作為一個靜態頁面伺服器啟動。

```
$ docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
```
使用者也可以不使用這種對應方式，透過利用 Dockerfile 來直接將靜態頁面內容放到映像檔中，內容為

```
FROM nginx
COPY static-html-directory /usr/share/nginx/html
```
之後產生新的映像檔，並啟動一個容器。

```
$ docker build -t some-content-nginx .
$ docker run --name some-nginx -d some-content-nginx
```
開放連接埠，並對應到本地的 `8080` 連接埠。

```
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

Nginx的 預設設定檔路徑為 `/etc/nginx/nginx.conf`，可以透過對應它來使用本地的設定檔，例如

```
docker run --name some-nginx -v /some/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx
```
使用設定檔時，為了在容器中正常執行，需要保持 `daemon off;`。

### Dockerfile
#### stable 版本
```
FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.10.1-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
	&& echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y \
						ca-certificates \
						nginx=${NGINX_VERSION} \
						nginx-module-xslt \
						nginx-module-geoip \
						nginx-module-image-filter \
						nginx-module-perl \
						nginx-module-njs \
						gettext-base \
	&& rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```

#### 1.11.x 版本

```
FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.11.1-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
	&& echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y \
						ca-certificates \
						nginx=${NGINX_VERSION} \
						nginx-module-xslt \
						nginx-module-geoip \
						nginx-module-image-filter \
						nginx-module-perl \
						nginx-module-njs \
						gettext-base \
	&& rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```
