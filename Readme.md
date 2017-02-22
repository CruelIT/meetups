# 22.02.17 - Docker - Kostya

## Анонс
Докеровцы. Напоминаю: собираемся в среду 7.00

заранее все скачайте и установите, т.к. с инетом на месте будут проблемы.

- поставьте докер + убедитесь, что у вас работает `docker run hello-world` (https://docs.docker.com/engine/installation/)
!!! версия минимум 1.13
- если `docker-compose -v` вернет ошибку, то надо его поставить: https://docs.docker.com/compose/install/
 
```
docker pull busybox
docker pull alpine:3.5
docker pull centos
docker pull node:boron
```

- если еще не читали мой пост про init, обязательно прочтите: https://vk.com/wall-134346952_293

с планом я определился. 3 часа примерно поровну поделим на 3 раздела: концепция, докерфайлы, админство.

## План

### 1 Концепция

+ проблема админ vs разраб
 - настройка среды
 - обновления (подмена)
 - замусоривание PATH

+ https://docs.docker.com/engine/understanding-docker/ картинки
 - image + dockerfile
 - container
 - volume vs host dir, volume cont, shared, relabel
 - networks

### 2 Dockerfile

- compose  https://docs.docker.com/compose/gettingstarted/
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
- alpine, debian, centos, node
- docker hub
- init особенности
- healthchecks
- registry
- gitlab ci
- docker-in-docker

### 3 Админство

- docker inspect
- cgroups - квоты
- devicemapper - https://docs.docker.com/engine/userguide/storagedriver/device-mapper-driver/
- tls
- nat
- docker proxy
- internal dns !=bridge
- логи
- swarm + k8s
- баги
 + kernel panic https://github.com/docker/docker/issues/5618
 + udp nat https://github.com/docker/docker/issues/11998
 + openvpn https://github.com/docker/libnetwork/issues/779
 + device or resource busy https://github.com/docker/docker/issues/22260
 + strongswan snat: короче хуево все с SNAT у докера. самый адекватный вариант это костыльнуть башик, который будет тыркать docker inspect и прописывать нужные SNAT правила. вопрос только чем его дергать. по сути надо его дергать на старт докера и на любое изменения докер сетей. хуков у докера нет никаких

- `docker system prune` https://github.com/docker/docker/pull/26108

## Телеграм

Kostya Esmukov, [22.02.17 08:20]  
https://docs.docker.com/compose/gettingstarted/#/step-2-create-a-dockerfile  

Kostya Esmukov, [22.02.17 08:25]  
```
// Load the http module to create an http server.
var http = require('http');

// Configure our HTTP server to respond with Hello World to all requests.
var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Hello World\n");
});

// Listen on port 8000, IP defaults to 127.0.0.1
server.listen(8000);

// Put a friendly message on the terminal
console.log("Server running at http://127.0.0.1:8000/");
```

Kostya Esmukov, [22.02.17 08:26]  
```
FROM node:boron
ADD . /app
WORKDIR /app
CMD ["node", "index.js"]
```

Kostya Esmukov, [22.02.17 08:27]  
`docker build -t hi .`

Kostya Esmukov, [22.02.17 08:31]  
`docker run -it --rm hi`

Kostya Esmukov, [22.02.17 08:54]  
`docker run -it --rm -p 81:8000 --init hi`

## Запись

https://drive.google.com/file/d/0B6eriKMc_luZUXVObmR3VDlER2M/view?usp=sharing

## Бумага

TODO

## Что не успели

- поиграться с busybox, alpine:3.5 и centos
- volume relabel (volume flags в docker run -- s, z, Z)
- compose
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
- `docker system prune` (https://github.com/docker/docker/pull/26108)
