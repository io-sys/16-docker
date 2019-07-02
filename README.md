# 16-task-Docker


### 1. Создайте свой кастомный образ `nginx` на базе `alpine`. 
После запуска `nginx` должен отдавать кастомную страницу (достаточно изменить дефолтную страницу `nginx`).  

__Dockerfile__
```php
FROM alpine:3.9

RUN set -x \
# Create nginx user/group first, to be consistent throughout docker variants
    && addgroup -g 101 -S nginx \
    && adduser -S -D -H -u 101 -h /var/cache/nginx -s /sbin/nologin -G nginx -g nginx nginx \
    && apk update \
    && apk upgrade\
    && apk add nginx \
        mc \
        vim \
        bash \
        curl \
    && mkdir -p /www/data \
    && mkdir -p /run/nginx \
    && chown -R nginx:nginx /var/lib/nginx \
    && chown -R nginx:nginx /www/data \
# Create index.html
    && echo "Default web page" > /www/data/index.html \
# Forward request and error logs to docker log collector
# variables.
    && chown -R nginx:nginx /var/log/nginx \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/

EXPOSE 80

STOPSIGNAL SIGTERM

USER nginx 

CMD ["nginx", "-g", "daemon off;"]
```
  
__nginx.conf__
```php
# https://wiki.alpinelinux.org/wiki/Nginx
user                            nginx;
worker_processes                auto; # it will be determinate automatically by the number of core

error_log                       /var/log/nginx/error.log warn;
#pid /var/run/nginx.pid; # it permit you to use /etc/init.d/nginx reload|restart|stop|start

events {
    worker_connections          1024;
}

http {
    include                     /etc/nginx/mime.types;
    default_type                application/octet-stream;
    sendfile                    on;
    access_log                  /var/log/nginx/access.log;
    keepalive_timeout           3000;
    server {
        listen                  80;
        root                    /www/data;
        index                   index.html index.htm;
        server_name             localhost;
        client_max_body_size    32m;
        error_page              500 502 503 504  /50x.html;
        location = /50x.html {
              root              /var/lib/nginx/html;
        }
    }
}
```

### 2. 
