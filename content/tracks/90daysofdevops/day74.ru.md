---
title: 74. Hello World - Jenkinsfile App Pipeline
description: 
toc: true
authors:
tags: [devops]
categories:
series: 
date: "2022-07-01"
lastmod: "2022-07-01"
featuredImage:
draft: false
id: 1048744
weight: 74
---

## Здравствуй мир - Jenkinsfile App Pipeline

В предыдущем разделе мы построили простой конвейер в Jenkins, который будет перемещать наш образ докера из нашего dockerfile в публичном репозитории GitHub в наш частный репозиторий Dockerhub.

В этом разделе мы хотим сделать еще один шаг вперед и добиться следующего с помощью нашего простого приложения.

### Цель

- Dockerfile (Hello World)
- Jenkinsfile
- Jenkins Pipeline для запуска при обновлении репозитория GitHub
- Используйте репозиторий GitHub в качестве источника.
- Запуск - Clone/Get Repository, Build, Test, Deploy Stages
- Развертывание на DockerHub с инкрементными номерами версий
- Stretch Goal для развертывания на нашем кластере Kubernetes (для этого потребуется еще одно задание и репозиторий манифеста с использованием учетных данных GitHub).

### Шаг первый

У нас есть наш [GitHub репозиторий](https://github.com/MichaelCade/Jenkins-HelloWorld) В настоящее время он содержит наш Dockerfile и наш index.html

![](../images/Day74_CICD1.ru.png?v1)

Это то, что мы использовали в качестве источника в нашем конвейере, теперь мы хотим добавить этот скрипт Jenkins Pipeline в наш репозиторий GitHub.

![](../images/Day74_CICD2.ru.png?v1)

Теперь вернемся к нашей приборной панели Jenkins и создадим новый пайплайн, но теперь вместо вставки нашего скрипта мы будем использовать "Pipeline script from SCM" Мы будем использовать приведенные ниже параметры конфигурации.

Для справки мы будем использовать <https://github.com/MichaelCade/Jenkins-HelloWorld.git> в качестве URL репозитория.  

![](../images/Day74_CICD3.ru.png?v1)

На этом этапе мы можем нажать кнопку сохранить и применить, после чего мы сможем вручную запустить наш Pipeline для сборки нового образа Docker, загруженного в наш репозиторий DockerHub.

Однако я также хочу убедиться, что мы установили расписание, по которому при каждом изменении нашего репозитория или исходного кода будет запускаться сборка. Мы можем использовать веб-крючки или запланированное извлечение.

Это важный момент, потому что если вы используете дорогостоящие облачные ресурсы для хранения конвейера и у вас много изменений в репозитории кода, то вы понесете большие расходы. Мы знаем, что это демонстрационная среда, поэтому я использую опцию "poll scm". (Также я считаю, что при использовании minikube мне не хватает возможности использовать webhooks)

![](../images/Day74_CICD4.ru.png?v1)

Одна вещь, которую я изменил со вчерашней сессии, это то, что теперь я хочу загружать изображение в публичный репозиторий, который в данном случае будет michaelcade1\90DaysOfDevOps, мой Jenkinsfile уже содержит это изменение. И из предыдущих разделов я удалил все существующие образы демо-контейнеров.

![](../images/Day74_CICD5.ru.png?v1)

Двигаясь назад, мы создали наш Pipeline, а затем, как было показано ранее, добавили нашу конфигурацию.

![](../images/Day74_CICD6.ru.png?v1)

На данном этапе наш конвейер еще не запущен, и вид сцены будет выглядеть примерно так.

![](../images/Day74_CICD7.ru.png?v1)

Теперь нажмем кнопку "Build Now". и в представлении этапа будут отображены наши этапы.

![](../images/Day74_CICD8.ru.png?v1)

Если мы перейдем к нашему репозиторию DockerHub, у нас должно быть 2 новых образа Docker. У нас должен быть идентификатор сборки 1 и последняя версия, потому что каждая сборка, которую мы создаем на основе команды "Upload to DockerHub", отправляет версию, используя переменную окружения Jenkins Build_ID, а также выпускает последнюю версию.

![](../images/Day74_CICD9.ru.png?v1)

Давайте создадим обновление файла index.html в нашем репозитории GitHub, как показано ниже, я позволю вам пойти и узнать, что говорила версия 1 файла index.html.

![](../images/Day74_CICD10.ru.png?v1)

Если мы вернемся в Jenkins и снова выберем "Build Now". Мы увидим, что наша сборка #2 прошла успешно.

![](../images/Day74_CICD11.ru.png?v1)

Затем быстро взглянув на DockerHub, мы увидим, что у нас есть наш тег версии 2 и наш последний тег.  

![](../images/Day74_CICD12.ru.png?v1)

Здесь стоит отметить, что я добавил в свой кластер Kubernetes секрет, который позволяет мне получить доступ и аутентификацию для отправки моих сборок docker в DockerHub. Если вы следуете этому примеру, вам следует повторить этот процесс для своей учетной записи, а также внести изменения в Jenkinsfile, связанный с моим репозиторием и учетной записью.

## Ресурсы

- [Jenkins is the way to build, test, deploy](https://youtu.be/_MXtbjwsz3A)
- [Jenkins.io](https://www.jenkins.io/)
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
- [ArgoCD Tutorial for Beginners](https://www.youtube.com/watch?v=MeU5_k9ssrs)
- [What is Jenkins?](https://www.youtube.com/watch?v=LFDrDnKPOTg)
- [Complete Jenkins Tutorial](https://www.youtube.com/watch?v=nCKxl7Q_20I&t=3s)
- [GitHub Actions](https://www.youtube.com/watch?v=R8_veQiYBjI)
- [GitHub Actions CI/CD](https://www.youtube.com/watch?v=mFFXuXjVgkU)