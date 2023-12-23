---
title: 72. Работа с Jenkins
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
id: 1048829
weight: 72
---

Сегодня мы планируем немного поработать с Jenkins и сделать что-то в рамках нашего конвейера CI, рассматривая некоторые примеры кодовых баз, которые мы можем использовать.

### Что такое конвейер?

Прежде чем мы начнем, нам нужно знать, что такое конвейер, когда речь идет о CI, и мы уже рассмотрели это на вчерашнем занятии с помощью следующего изображения.

![](../images/Day71_CICD4.ru.png?v1)

Мы хотим взять процессы или шаги, описанные выше, и автоматизировать их, чтобы в итоге получить результат, то есть развернутое приложение, которое мы можем отправить нашим клиентам, конечным пользователям и т.д.

Этот автоматизированный процесс позволяет нам иметь контроль версий для наших пользователей и клиентов. Каждое изменение, улучшение функций, исправление ошибок и т.д. проходит через этот автоматизированный процесс, подтверждая, что все в порядке, без излишнего ручного вмешательства, чтобы убедиться, что наш код хорош.

Этот процесс включает в себя создание программного обеспечения надежным и повторяемым способом, а также продвижение созданного программного обеспечения (называемого "сборкой") через несколько этапов тестирования и развертывания.

Конвейер jenkins записывается в текстовый файл Jenkinsfile. Который сам должен быть зафиксирован в репозитории контроля исходного кода. Это также известно как Pipeline as code, мы также можем сравнить это с Infrastructure as code, о которой мы рассказывали несколько недель назад.

[Jenkins Pipeline Definition](https://www.jenkins.io/doc/book/pipeline/#ji-toolbar)

### Развертывание Jenkins

Я получил некоторое удовольствие от развертывания Jenkins, Вы заметите из [документации](https://www.jenkins.io/doc/book/installing/), что есть много вариантов того, где вы можете установить Jenkins.

Учитывая, что у меня под рукой есть minikube, и мы уже использовали его несколько раз, я хотел использовать его и для этой задачи. (Хотя шаги, описанные в [Kubernetes Installation](https://www.jenkins.io/doc/book/installing/kubernetes/), привели к тому, что я уперся в стену и не смог запустить систему, вы можете сравнить эти два варианта, когда я задокументирую свои шаги здесь.

Первым шагом будет запуск нашего кластера minikube, мы можем сделать это с помощью команды `minikube start`.

![](../images/Day72_CICD1.ru.png?v1)

Я добавил папку со всеми конфигурациями и значениями YAML, которые можно найти [здесь](../CICD/Jenkins) Теперь, когда у нас есть наш кластер, мы можем выполнить следующие действия для создания пространства имен jenkins. `kubectl create -f jenkins-namespace.yml`

![](../images/Day72_CICD2.ru.png?v1)

Мы будем использовать Helm для развертывания jenkins в нашем кластере, о Helm мы рассказывали в разделе Kubernetes. Сначала нам нужно добавить репозиторий jenkinsci в helm `helm repo add jenkinsci https://charts.jenkins.io`, затем обновить наши таблицы `helm repo update`.

![](../images/Day72_CICD3.ru.png?v1)

Идея Jenkins заключается в том, что он будет сохранять состояние для своих пайплайнов, вы можете запустить вышеупомянутую установку helm без персистентности, но если эти pods будут перезагружены, изменены или модифицированы, то все пайплайны или конфигурации, которые вы создали, будут потеряны. Мы создадим том для персистентности, используя файл jenkins-volume.yml с помощью команды `kubectl apply -f jenkins-volume.yml`.

![](../images/Day72_CICD4.ru.png?v1)

Нам также нужна учетная запись службы, которую мы можем создать с помощью этого yaml-файла и команды. `kubectl apply -f jenkins-sa.yml`

![](../images/Day72_CICD5.ru.png?v1)

На этом этапе мы готовы к развертыванию с помощью схемы helm, сначала мы определим нашу схему с помощью `chart=jenkinsci/jenkins`, а затем развернем с помощью этой команды, где jenkins-values.yml содержит учетные записи персистентности и сервисов, которые мы ранее развернули на нашем кластере. `helm install jenkins -n jenkins -f jenkins-values.yml $chart`.

![](../images/Day72_CICD6.ru.png?v1)
На этом этапе наши капсулы будут извлекать образ, но у капсулы не будет доступа к хранилищу, поэтому никакая конфигурация не может быть начата с точки зрения запуска Jenkins.

Именно здесь документация не помогла мне понять, что должно произойти. Но мы видим, что у нас нет разрешения на запуск установки jenkins.

![](../images/Day72_CICD7.ru.png?v1)

Для того чтобы исправить вышеописанное или решить проблему, нам нужно убедиться, что мы предоставили доступ или правильное разрешение для того, чтобы наши jenkins pods могли писать в это место, которое мы предложили. Мы можем сделать это, используя `minikube ssh`, который введет нас в докер-контейнер minikube, на котором мы работаем, а затем, используя `sudo chown -R 1000:1000 /data/jenkins-volume`, мы можем убедиться, что у нас установлены разрешения на наш том данных.

![](../images/Day72_CICD8.ru.png?v1)

Вышеописанный процесс должен исправить капсулы, однако если это не так, вы можете заставить капсулы обновиться с помощью команды `kubectl delete pod jenkins-0 -n jenkins`. На этом этапе у вас должно быть 2/2 запущенных стручка под названием jenkins-0.

![](../images/Day72_CICD9.ru.png?v1)

Теперь нам нужен наш пароль администратора, и мы можем сделать это с помощью следующей команды. `kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/chart-admin-password && echo`

![](../images/Day72_CICD10.ru.png?v1)

Теперь откройте новый терминал, так как мы собираемся использовать команду `port-forward`, чтобы получить доступ с нашей рабочей станции. `kubectl --namespace jenkins port-forward svc/jenkins 8080:8080`.

![](../images/Day72_CICD11.ru.png?v1)

Теперь мы должны быть в состоянии открыть браузер и войти на <http://localhost:8080> и аутентифицироваться с именем пользователя: admin и паролем, которые мы собрали в предыдущем шаге.

![](../images/Day72_CICD12.ru.png?v1)

После аутентификации наша страница приветствия Jenkins должна выглядеть примерно так:

![](../images/Day72_CICD13.ru.png?v1)

Отсюда я бы предложил перейти к "Manage Jenkins", и вы увидите "Manage Plugins", где будут доступны некоторые обновления. Выберите все эти плагины и выберите "Загрузить сейчас и установить после перезапуска".

![](../images/Day72_CICD14.ru.png?v1)

Если вы хотите пойти еще дальше и автоматизировать развертывание Jenkins с помощью shell-скрипта, этот замечательный репозиторий был предоставлен мне в twitter [mehyedes/nodejs-k8s](https://github.com/mehyedes/nodejs-k8s/blob/main/docs/automated-setup)

### Jenkinsfile

Теперь у нас есть Jenkins, развернутый в нашем кластере Kubernetes, мы можем вернуться назад и подумать об этом Jenkinsfile.

Каждый Jenkinsfile, скорее всего, будет начинаться примерно так: сначала вы определяете шаги вашего конвейера, в данном случае это Build > Test > Deploy. Но на самом деле мы не делаем ничего, кроме использования команды `echo` для вызова определенных этапов.

```

Jenkinsfile (декларативный конвейер)
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

```

В нашей приборной панели Jenkins выберите "New Item" дайте элементу имя, я собираюсь "echo1" Я собираюсь предложить, что это Pipeline.

![](../images/Day72_CICD15.ru.png?v1)

Нажмите Ok, и у вас появятся вкладки (General, Build Triggers, Advanced Project Options и Pipeline) для простого теста нас интересует только Pipeline. В разделе Pipeline у вас есть возможность добавить скрипт, мы можем скопировать и вставить приведенный выше скрипт в поле.

Как мы уже говорили выше, это не даст многого, но покажет нам этапы нашей сборки > тестирования > развертывания

![](../images/Day72_CICD16.ru.png?v1)

Нажмите Save, теперь мы можем запустить нашу сборку, используя сборку, показанную ниже.

![](../images/Day72_CICD17.ru.png?v1)

Мы также должны открыть терминал и выполнить команду `kubectl get pods -n jenkins`, чтобы посмотреть, что произойдет.

![](../images/Day72_CICD18.ru.png?v1)

Хорошо, очень просто, но теперь мы можем видеть, что наше развертывание и установка Jenkins работает правильно, и мы можем начать видеть здесь строительные блоки конвейера CI.

В следующем разделе мы будем строить конвейер Jenkins.

## Ресурсы

- [Jenkins is the way to build, test, deploy](https://youtu.be/_MXtbjwsz3A)
- [Jenkins.io](https://www.jenkins.io/)
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
- [ArgoCD Tutorial for Beginners](https://www.youtube.com/watch?v=MeU5_k9ssrs)
- [What is Jenkins?](https://www.youtube.com/watch?v=LFDrDnKPOTg)
- [Complete Jenkins Tutorial](https://www.youtube.com/watch?v=nCKxl7Q_20I&t=3s)
- [GitHub Actions](https://www.youtube.com/watch?v=R8_veQiYBjI)
- [GitHub Actions CI/CD](https://www.youtube.com/watch?v=mFFXuXjVgkU)