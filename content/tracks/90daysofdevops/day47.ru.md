---
title: 47. Сетевое взаимодействие Docker и безопасность
description: 
toc: true
authors:
tags: [devops]
categories:
series: 
date: "2022-06-06"
lastmod: "2022-06-06"
featuredImage:
draft: false
id: 1049078
weight: 47
---
## Docker Networking & Security

Во время этой сессии по контейнерам мы уже кое-что сделали, но не рассмотрели, как все работает за кулисами с точки зрения сетевых технологий, а также не затронули безопасность, поэтому мы планируем эту сессию.

### Основы сетевого взаимодействия Docker

Откройте терминал и введите команду `docker network` - это основная команда для настройки и управления сетями контейнеров.

Ниже показано, как мы можем использовать эту команду и все доступные подкоманды. Мы можем создавать новые сети, составлять список существующих, проверять и удалять сети.

![](../images/Day47_Containers1.ru.png?v1)

Давайте посмотрим на существующие сети, которые у нас есть с момента установки, поэтому из коробки Docker networking выглядит как использование команды `docker network list`.

Каждая сеть получает уникальный ID и NAME. Каждая сеть также связана с одним драйвером. Обратите внимание, что сеть "bridge" и сеть "host" имеют те же имена, что и их соответствующие драйверы.

![](../images/Day47_Containers2.ru.png?v1)

Далее мы можем более детально рассмотреть наши сети с помощью команды `docker network inspect`.

Запустив команду `docker network inspect bridge`, я могу получить все детали конфигурации конкретного имени сети. Сюда входят имя, ID, драйверы, подключенные контейнеры и, как вы можете видеть, многое другое.

![](../images/Day47_Containers3.ru.png?v1)

### Docker: Bridge Networking

Как вы видели выше, стандартная установка Docker Desktop дает нам предварительно созданную сеть под названием `bridge` Если вы обратитесь к команде `docker network list`, то увидите, что сеть под названием bridge связана с драйвером `bridge`. То, что у них одинаковое имя, не означает, что это одно и то же. Связаны, но не одно и то же.

Вывод выше также показывает, что сеть bridge имеет локальную привязку. Это означает, что сеть существует только на этом хосте Docker. Это справедливо для всех сетей, использующих драйвер моста - драйвер моста обеспечивает работу сети на одном хосте.

Все сети, созданные с помощью драйвера моста, основаны на мосте Linux (он же виртуальный коммутатор).

### Подключение контейнера

По умолчанию новым контейнерам назначается сеть bridge, то есть, если вы не укажете сеть, все контейнеры будут подключены к сети bridge.

Давайте создадим новый контейнер командой `docker run -dt ubuntu sleep infinity`.

Команда sleep выше просто будет поддерживать работу контейнера в фоновом режиме, чтобы мы могли возиться с ним.

![](../images/Day47_Containers4.ru.png?v1)

Если мы затем проверим нашу сеть моста с помощью `docker network inspect bridge`, вы увидите, что у нас есть контейнер, соответствующий тому, что мы только что развернули, потому что мы не указали сеть.

![](../images/Day47_Containers5.ru.png?v1)

Мы также можем погрузиться в контейнер, используя `docker exec -it 3a99af449ca2 bash`, вам придется использовать `docker ps`, чтобы получить идентификатор контейнера.

Отсюда наш образ не имеет ничего для пинга, поэтому нам нужно выполнить следующую команду.`apt-get update && apt-get install -y iputils-ping` затем пингуем внешний адрес интерфеса. `ping -c5 www.90daysofdevops.com`

![](../images/Day47_Containers6.ru.png?v1)

Чтобы устранить эту проблему, мы можем запустить `docker stop 3a99af449ca2` и снова использовать `docker ps` для поиска ID вашего контейнера, но это приведет к удалению нашего контейнера.

### Настройте NAT для внешнего подключения

На этом шаге мы запустим новый контейнер NGINX и назначим порт 8080 на хосте Docker на порт 80 внутри контейнера. Это означает, что трафик, поступающий на хост Docker по порту 8080, будет передаваться на порт 80 внутри контейнера.

Запустите новый контейнер на основе официального образа NGINX, выполнив команду `docker run --name web1 -d -p 8080:80 nginx`.

![](../images/Day47_Containers7.ru.png?v1)

Просмотрите состояние контейнера и сопоставление портов, выполнив команду `docker ps`.

![](../images/Day47_Containers8.ru.png?v1)

Верхняя строка показывает новый контейнер web1, запущенный NGINX. Обратите внимание на команду, которую запускает контейнер, а также на сопоставление портов - `0.0.0.0:8080->80/tcp` сопоставляет порт 8080 на всех интерфейсах хоста с портом 80 внутри контейнера web1. Это сопоставление портов делает веб-сервис контейнера доступным из внешних источников (через IP-адрес хоста Docker на порту 8080).

Теперь нам нужен IP-адрес нашего реального хоста, мы можем сделать это, зайдя в терминал WSL и используя команду `ip addr`.

![](../images/Day47_Containers9.ru.png?v1)

Затем мы можем взять этот IP, открыть браузер и перейти по адресу `http://172.25.218.154:8080/` Ваш IP может быть другим. Это подтверждает, что NGINX доступен.

![](../images/Day47_Containers10.ru.png?v1)

Я взял эти инструкции с этого сайта с далекого 2017 DockerCon, но они актуальны и сегодня. Однако остальная часть руководства посвящена Docker Swarm, и я не собираюсь рассматривать его здесь. [Docker Networking - DockerCon 2017](https://github.com/docker/labs/tree/master/dockercon-us-2017/docker-networking)

### Обеспечение безопасности контейнеров

Контейнеры обеспечивают безопасную среду для рабочих нагрузок по сравнению с полной конфигурацией сервера. Они позволяют разбить ваши приложения на более мелкие, слабо связанные компоненты, изолированные друг от друга, что помогает уменьшить поверхность атаки в целом.

Но они не застрахованы от хакеров, которые хотят использовать системы в своих целях. Нам по-прежнему необходимо понимать подводные камни безопасности этой технологии и придерживаться лучших практик.

### Откажитесь от прав root

Все контейнеры, которые мы развернули, использовали права root для процессов внутри контейнеров. Это означает, что они имеют полный административный доступ к вашим контейнерам и хост-средам. Теперь для целей прохождения мы знали, что эти системы не будут работать долго. Но вы видели, как легко их запустить.

Мы можем добавить несколько шагов к нашему процессу, чтобы дать возможность не root-пользователям быть предпочтительной лучшей практикой. При создании нашего dockerfile мы можем создать учетные записи пользователей. Вы можете найти этот пример также в папке containers в репозитории.

```
# Используем официальную версию Ubuntu 18.04 в качестве базовой
FROM ubuntu:18.04
RUN apt-get update && apt-get upgrade -y
RUN groupadd -g 1000 basicuser && useradd -r -u 1000 -g basicuser basicuser
пользователь basicuser
```

Мы также можем использовать `docker run --user 1009 ubuntu` Команда Docker run переопределяет любого пользователя, указанного в вашем Dockerfile. Поэтому в следующем примере ваш контейнер всегда будет запускаться с наименьшими привилегиями при условии, что идентификатор пользователя 1009 также имеет самый низкий уровень прав.

Однако этот метод не устраняет основной недостаток безопасности самого образа. Поэтому лучше указать в Dockerfile пользователя, не являющегося root, чтобы ваши контейнеры всегда запускались безопасно.

### Частный репозитории

Еще одна область, которую мы активно используем, - это публичные реестры в DockerHub, а частный реестр образов контейнеров, созданный вашей организацией, означает, что вы можете размещать их там, где пожелаете, или же для этого существуют управляемые сервисы, но в целом это дает вам полный контроль над образами, доступными для вас и вашей команды.

DockerHub отлично подходит для создания базового уровня, но он предоставляет только базовый сервис, где вам придется во многом доверять издателю образа.

### Lean & Clean

Мы уже упоминали об этом, хотя это и не связано с безопасностью. Но размер вашего контейнера также может влиять на безопасность с точки зрения поверхности атаки, если у вас есть ресурсы, которые вы не используете в своем приложении, то они не нужны в вашем контейнере.

Это также является моей основной проблемой при использовании `последних` образов, потому что это может принести много лишнего в ваши образы. DockerHub показывает сжатый размер для каждого образа в хранилище.

`docker image` - отличная команда для просмотра размера ваших образов.  

![](../images/Day47_Containers11.ru.png?v1)

## Ресурсы

- [TechWorld with Nana - Docker Tutorial for Beginners](https://www.youtube.com/watch?v=3c-iBn73dDE)
- [Programming with Mosh - Docker Tutorial for Beginners](https://www.youtube.com/watch?v=pTFZFxd4hOI)
- [Docker Tutorial for Beginners - What is Docker? Introduction to Containers](https://www.youtube.com/watch?v=17Bl31rlnRM&list=WL&index=128&t=61s)
- [WSL 2 with Docker getting started](https://www.youtube.com/watch?v=5RQbdMn04Oc)
- [Blog on gettng started building a docker image](https://stackify.com/docker-build-a-beginners-guide-to-building-docker-images/)
- [Docker documentation for building an image](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [YAML Tutorial: Everything You Need to Get Started in Minute](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started)