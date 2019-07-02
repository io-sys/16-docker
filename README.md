# 16-task-Docker


### 1. Создайте свой кастомный образ `nginx` на базе `alpine`. 
После запуска `nginx` должен отдавать кастомную страницу (достаточно изменить дефолтную страницу `nginx`).  

Папка с решением `docker-nginx`, файлы:
1. `Dockerfile`
2. `nginx.conf`  
или репозиторий в `Docker Hub`.  

__Проверка__  
Загрузить образ из `Docker Hub` репозитория.  
`docker pull yamondej/nginxcustom:alpine`  
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


### 2. 
