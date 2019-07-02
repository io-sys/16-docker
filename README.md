# 16-task-Docker


### 1. Создайте свой кастомный образ `nginx` на базе `alpine`. 
После запуска `nginx` должен отдавать кастомную страницу (достаточно изменить дефолтную страницу `nginx`).  

_Dockerfile
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

### 2. 
