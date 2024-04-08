# Задание: Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)

## Решение:

---

Создаём Dockerfile следующего содержания:

```
FROM alpine
LABEL maintainer="Sarajkin Andrej a.sarajkin@trs58.ru"

ENV TZ=Europe/Moscow

RUN apk update && apk upgrade && apk add nginx
COPY ./html/html /var/www/html/default/
COPY ./html/default.conf  /etc/nginx/http.d/default.conf 

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Создём образ на основе Alpine:

`docker build -f ./DockerfileNginxAlpine -t nargamard/nginx .`

Запускпеми контейнер на основе созданного образа и пробраываем порт 80:

`docker run -dti -p 80:80 nargamard/nginx`

Смотрим запущенные контейнеры:

```bash
[andrej@home-srv OTUS-Task13]$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                               NAMES
eeef65501327   nargamard/nginx   "nginx -g 'daemon of…"   15 minutes ago   Up 15 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   flamboyant_swanson
```

В браузере открываем http://localhost:80 и читаем на нашей кастомной странице о Достоевском:

![Alt text](%D0%A1%D0%BA%D1%80%D0%B8%D0%BD_html.png)

Теперь запушим образ на DockerHub:

```bash
[andrej@home-srv OTUS-Task13]$ docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/andrej/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

[andrej@home-srv OTUS-Task13]$ docker push nargamard/nginx:latest 
The push refers to repository [docker.io/nargamard/nginx]
ae3e03d12c70: Pushed 
cf9e25b3429b: Pushed 
1edce115ac40: Pushed 
d4fc045c9e3a: Mounted from library/alpine 
latest: digest: sha256:ed5171730883a360f870eb66cd216f9fa2486994ea5c38ab32d3255a76f51505 size: 1155

```
Ссылка на образ на DockerHub: `https://hub.docker.com/repository/docker/nargamard/nginx`

---

## Задание: 
Определите разницу между контейнером и образом, вывод опишите в домашнем задании.

## Ответ:
Образ - это шаблон, на основании которого будут запускаться контейнеры, представляет собой среду выполнения, куда могут быть дополнительно установлены нужные пакеты, добавлены файлы конфигурации сервисов.

Контейнер - это работающий экземпляр образа. На основе одного образа могут быть запущены несколько контейнеров.

## Ответьте на вопрос
Можно ли в контейнере собрать ядро?

## Ответ:
Можно. Во всяком случае не встретилось информации, почему это нельзя сделать. 
