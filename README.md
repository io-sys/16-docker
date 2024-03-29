# 15-task-Docker


### 1. Создайте свой кастомный образ `nginx` на базе `alpine`. 
После запуска `nginx` должен отдавать кастомную страницу (достаточно изменить дефолтную страницу `nginx`).  

Папка с решением [`docker-nginx`](https://github.com/io-sys/16-docker/tree/master/docker-nginx), файлы:
1. [`Dockerfile`](https://github.com/io-sys/16-docker/blob/master/docker-nginx/Dockerfile)
2. [`nginx.conf`](https://github.com/io-sys/16-docker/blob/master/docker-nginx/nginx.conf)  
или репозиторий в `Docker Hub` с образом https://hub.docker.com/r/yamondej/nginxcustom  

__Проверка__  
Загрузить образ из `Docker Hub` репозитория.  
```php
docker pull yamondej/nginxcustom:alpine
```  
Запустить контейнер из скаченного образа с `Docker Hub`.  
```php
docker run -d --restart=unless-stopped -p 80:80 yamondej/nginxcustom:alpine
```
Проверить запущенные контейнеры.  
```php
docker ps
```
Браузер http://192.168.11.150/  
`Default web page`


### 2. Определите разницу между контейнером и образом.
__Контейнер__  
Один `Docker`-контейнер = 1 сервис, сами контейнеры создаются из образов `image`. Так же можно внести изменения в работающем контенере, потом закоммитить его и получить новый образ.
```php
docker commit 6e6829ad513e nginxcustom:v1.1
```

__Образ__  
Контейнеры создаются из образов, образ используется в качестве шаблона для создания контейнеров. Несколько контейнеров могут быть запущены, используя один и тот же образ.
Сам по себе `Docker`-образ невозможно «запускать», запускаются контейнеры, внутри которых работает приложение. Образы можно хранить в `Docker` `Repository`, например, `Docker Hub` или `GitLab`, откуда образы можно загрузить на хостовую систему.  
Образ собирается из:  
  1) слоев, каждый слой собирается из своей директивы в `Dockerfile`'е, где директивы `ENV` и `ARG` не являются слоями и  
  2) начального образа, болванки, из директивы `FROM` в `Dockerfile`'е.  

Всего слоев может быть 127(122), слои внутри образа `R/O` только на чтение, слои образа идемпотентны, это упрощает сборку образа из `Dockerfile`'ов. Например,  когда необходимо внести изменения только в один слой, изменить 1'у директиву в `Dockerfile`'е, и пересобрать образ, слои которые не правились будут использованы повторно в сборке `build`, такая сборка образа пройдет быстрее её первоначальной сборки.

### 3. Можно ли в контейнере собрать ядро?  
В `Docker`-контейнере можно собрать ядро с произвольными патчами, флагами конфигурации и тегом, например, `repository` на сайте `Docker Hub` для сборки ядра `Debian` по ссылкам:  
hub https://hub.docker.com/r/tomzo/buildkernel  
git https://github.com/tomzo/docker-kernel-ide  
  
Быстрым поиском в `Google` нашлись 3'и `repository`, где собирают ядро `Linux` под разные `Linux` дистрибутивы внутри `Docker`-контейнера.

### 4. Задание со * (звездочкой) 
Создайте кастомные образы `nginx` и `php`, объедините их в `docker-compose`. 
После запуска `nginx` должен показывать `php info`. 
Все собранные образы должны быть в `docker hub`.

Папка с решением [`docker-nginx-hub`](https://github.com/io-sys/16-docker/tree/master/docker-compose-hub) https://github.com/io-sys/16-docker/tree/master/docker-compose-hub

```php
docker-compose build
docker-compose up
Ctrl+C
docker-compose start
```
