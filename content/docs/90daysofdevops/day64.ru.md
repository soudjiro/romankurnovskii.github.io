---
title: 64. Ansible Введение
description: 
toc: true
authors:
tags: [devops]
categories:
series: 
date: "2022-06-23"
lastmod: "2022-06-23"
featuredImage:
draft: false
id: 1048765
weight: 64
---

Основы Ansible
<!--more-->
## Ansible: Начало работы

Мы немного рассказали о том, что такое Ansible, на вчерашней [большой сессии](../day63), но здесь мы собираемся начать с более подробной информации. Во-первых, Ansible поставляется компанией RedHat. Во-вторых, это агент, подключается через SSH и выполняет команды. В-третьих, он кроссплатформенный (Linux & macOS, WSL2) и с открытым исходным кодом (есть также платный корпоративный вариант) Ansible толкает конфигурацию по сравнению с другими моделями. 

### Установка Ansible 
Как вы можете себе представить, RedHat и команда Ansible проделали фантастическую работу по документированию Ansible. Обычно это начинается с шагов по установке, которые вы можете найти [здесь](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html). Помните, мы говорили, что Ansible - это инструмент автоматизации без агентов, инструмент развертывается на системе, называемой "узел управления", с этого узла управления осуществляется управление машинами и другими устройствами (возможно, сетевыми) по SSH. 

В документации по ссылке выше говорится, что ОС Windows не может использоваться в качестве узла управления. 

Для моего узла управления и, по крайней мере, для этой демонстрации я собираюсь использовать виртуальную машину Linux, которую мы создали еще в [разделе Linux](../day20) в качестве узла управления. 

Эта система работала под управлением Ubuntu, и для ее установки достаточно выполнить следующие команды. 

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```
Теперь у нас должна быть установлена ansible на нашем узле управления, вы можете проверить это, запустив `ansible --version`, и вы должны увидеть что-то похожее на это ниже. 

![](../images/Day64_config1.png?v1)

Прежде чем мы перейдем к управлению другими узлами в нашей среде, мы также можем проверить функциональность ansible, выполнив команду на нашей локальной машине `ansible localhost -m ping` будет использовать [Ansible Module](https://docs.ansible.com/ansible/2.9/user_guide/modules_intro.html), и это быстрый способ выполнить одну задачу на многих различных системах. Я имею в виду, что это не очень весело только с локальным хостом, но представьте, что вы хотите получить что-то или убедиться, что все ваши системы работают, а у вас 1000+ серверов и устройств. 

![](../images/Day64_config2.png?v1)

Или реальное использование модуля в реальной жизни может быть чем-то вроде `ansible webservers --m service -a "name=httpd state=started"`, это скажет нам, запущена ли служба httpd на всех наших веб-серверах. Я привел термин webservers, используемый в этой команде. 

### hosts 

Как я использовал localhost выше для запуска простого модуля ping против системы, я не могу указать другую машину в моей сети, например, в среде, которую я использую, мой хост Windows, на котором работает VirtualBox, имеет сетевой адаптер с IP 10.0.0.1, но вы можете видеть ниже, что я могу связаться с ним с помощью ping, но я не могу использовать ansible для выполнения этой задачи. 

![](../images/Day64_config3.png?v1)
Для того чтобы указать наши узлы или узлы, которые мы хотим автоматизировать с помощью этих задач, нам необходимо их определить. Мы можем определить их, перейдя в каталог /etc/ansible в вашей системе. 

![](../images/Day64_config4.png?v1)

Файл, который мы хотим отредактировать - это файл hosts, используя текстовый редактор, мы можем зайти в него и определить наши хосты. Файл hosts содержит множество отличных инструкций по использованию и изменению файла. Мы хотим прокрутить вниз и создать новую группу под названием [windows] и добавить наш IP-адрес `10.0.0.1` для этого хоста. Сохраните файл. 

![](../images/Day64_config5.png?v1)

Однако помните, я говорил, что вам понадобится SSH, чтобы Ansible мог подключиться к вашей системе. Как вы можете видеть ниже, когда я запускаю `ansible windows -m ping`, мы получаем недостижимый результат, потому что не удалось подключиться через SSH. 

![](../images/Day64_config6.png?v1)

Теперь я также начал добавлять дополнительные хосты в наш инвентарь, другое название для этого файла, так как здесь вы собираетесь определить все ваши устройства, это могут быть сетевые устройства, например, коммутаторы и маршрутизаторы, которые также будут добавлены сюда и сгруппированы. В нашем файле hosts я также добавил свои учетные данные для доступа к группе систем linux. 

![](../images/Day64_config7.png?v1)

Теперь, если мы запустим `ansible linux -m ping`, мы получим успех, как показано ниже. 

![](../images/Day64_config8.png?v1)

Далее у нас есть требования к узлам, это целевые системы, на которых вы хотите автоматизировать конфигурацию. Мы не устанавливаем на них ничего для Ansible (то есть, мы можем установить программное обеспечение, но нам не нужен клиент Ansible). Ansible будет устанавливать соединение по SSH и отправлять все по SFTP (если вы хотите и у вас настроен SSH, вы можете использовать SCP против SFTP). 

### Команды Ansible 

Вы видели, что мы смогли запустить `ansible linux -m ping` на нашей Linux машине и получить ответ, в принципе, с Ansible у нас есть возможность запускать множество специальных команд. Но очевидно, что вы можете запустить это против группы систем и получить эту информацию обратно. [ad hoc commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)

Если вы сталкиваетесь с повторением команд или, что еще хуже, вам приходится входить в отдельные системы для выполнения этих команд, то Ansible может помочь в этом случае. Например, простая команда ниже даст нам вывод всех сведений об операционной системе для всех систем, которые мы добавим в нашу группу linux. 
`ansible linux -a "cat /etc/os-release"`.

Другими вариантами использования могут быть перезагрузка систем, копирование файлов, управление упаковщиками и пользователями. Вы также можете объединить специальные команды с модулями Ansible. 

Специальные команды используют декларативную модель, рассчитывая и выполняя действия, необходимые для достижения заданного конечного состояния. Они достигают идемпотентности, проверяя текущее состояние перед началом работы и ничего не делая, если текущее состояние не отличается от заданного конечного состояния.

# Ресурсы 

- [What is Ansible](https://www.youtube.com/watch?v=1id6ERvfozo)
- [Ansible 101 - Episode 1 - Introduction to Ansible](https://www.youtube.com/watch?v=goclfp6a2IQ)
- [NetworkChuck - You need to learn Ansible right now!](https://www.youtube.com/watch?v=5hycyr-8EKs&t=955s)
