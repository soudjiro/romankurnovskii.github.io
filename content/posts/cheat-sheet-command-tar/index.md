---
title: Шпаргалка tar архиватор
description: Необходиые команды для работы с архиватором tar
toc: true
authors:
  - roman-kurnovskii
tags:
  ["tar", ]
categories: ['Code Snippets']
series: ['bash']
date: "2022-06-05"
lastmod: "2022-06-05"
featuredImage: /posts/cheat-sheet-command-tar/featured.jpg
draft: false
---

## Кратко

Создать:
```bash
tar cf archive.tar directory
```

Распаковать:
```bash
tar xf archive.tar
 ```

## Создание

```bash
mkdir my_dir # Создаем папку
tar cf dir_archive.tar my_dir # Создаем архив с папкой
ll # Проверяем содержимое текущего каталога
# -rw-r--r--  1 r  staff   1.5K Jun  4 14:42 dir_archive.tar
# drwxr-xr-x  2 r  staff    64B Jun  4 14:42 my_dir
```

## Распаковка

```bash
tar xf dir_archive.tar
```

## Сжатие

```bash
tar czf dir_archive.tar.gz dir_archive.tar
```

## Распаковка сжатого файла

```bash
tar xzf dir_archive.tar.gz
```

## Сжатие с помощью bzip2

```bash
tar cjf dir_archive.tar.bz2 my_dir
```

## Распаковка с помощью bzip2

```bash
tar xjf dir_archive.tar.bz2
```

## Просмотр содержимого архива

```bash
tar -tvf dir_archive.tar
```