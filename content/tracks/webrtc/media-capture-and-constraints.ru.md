---
title: Захват мультимедиа и ограничения
description: Карманная книга по WebRTC
toc: true
authors:
tags: 
categories:
series:
featuredImage:
date: "2022-07-02"
lastMod: "2022-06-28"
draft: false
weight: 3
---


### Захват мультимедиа и ограничения

Мультимедиа-часть WebRTC показывает, как получить доступ к оборудованию, способному записывать видео и аудио (например, камеры и микрофоны), а также как работают медиа-потоки. И помимо этого – средства отображения, которые позволяют делать захват экрана.

## Мультимедиа-устройства

Все камеры и микрофоны, поддерживаемые браузером, доступны и управляются через объект navigator.mediaDevices. Приложения могут получать текущий список подсоединенных устройств и отслеживать изменения, т.к. многие камеры и микрофоны подсоединены через USB, и могут подключаться/отключаться в течение работы приложения. Поскольку статус мультимедиа-устройства может меняться в любой момент времени, рекомендуем, чтоб приложения регистрировали все изменения в статусе устройства для правильной обработки статусов изменений.

## Ограничения

При получении доступа к мультимедиа-устройствам, хорошо бы обеспечить настолько подробные ограничения, насколько это возможно. И хотя можно открыть камеру и микрофон по умолчанию с простым ограничением, это может привести к тому, что медиапоток будет далеко не самым оптимальным для приложения.

Конкретные ограничения определяются в объекте `MediaTrackConstraint` (одно для аудио, одно для видео). Атрибуты в этом объекте типа `ConstraintLong`, `ConstraintBoolean`, `ConstraintDouble` или `ConstraintDOMString`. Данные могут быть как конкретным значением (например, число, Boolean или String), диапазоном (LongRange или DoubleRange с минимальным и максимальным значением) или объектом c ideal или exact определением. Для конкретных значений браузер будет пытаться выбрать что-то наиболее близкое. Для диапазонных будет использоваться лучшее значение из диапазона. Для exact – будет передаваться только тот медиа-поток, который точно соответствует заданным ограничениям.

```javascripton
NEAR
// Camera with a resolution as close to 640x480 as possible
{
    "video": {
        "width": 640,
        "height": 480
    }
}

RANGE
// Camera with a resolution in the range 640x480 to 1024x768
{
    "video": {
        "width": {
            "min": 640,
            "max": 1024
        },
        "height": {
            "min": 480,
            "max": 768
        }
    }
}

EXACT
// Camera with the exact resolution of 1024x768
{
    "video": {
        "width": {
            "exact": 1024
        },
        "height": {
            "exact": 768
        }
    }
}
```

Чтобы определить актуальную конфигурацию конкретной дорожки медиа-потока, мы можем воспользоваться запросом `MediaStreamTrack.getSettings()`, который возвращает набор настроек **MediaTrackSettings**, используемых в данные момент.

Также можно обновить ограничения дорожки с мультимедиа-устройства, которое открываем через `applyConstraints()`. Это позволяет приложению перенастроить устройство без прерывания текущего потока.

## Захват экрана

Приложение, которое потенциально может выполнять захват и запись экрана, должно использовать **Display Media API**. Функция `getDisplayMedia()` (которая является частью navigator.mediaDevices), аналогична `getUserMedia()` и используется, чтобы открыть содержимое дисплея (или его части, например, окна). Возвращенный **MediaStream** работает также, как при использовании `getUserMedia()`.

Ограничения для getDisplayMedia() отличаются от ограничений, используемых для обычных входящих видео- и аудио-потоков.

```javascripton
{
    video: {
        cursor: ‘always’ | ‘motion’ | ‘never’,
        displaySurface: ‘application’ | ‘browser’ | ‘monitor’ | ‘window’
    }
}
```

Фрагмент кода выше показывает, как работают специальные ограничения для записи экрана. Обратите внимание, что они могут не поддерживаться некоторыми браузерами, поддерживающими отображение мультимедиа.

## Потоки и дорожки

MediaStream представляет собой поток медиаконтента, который состоит из аудио- и видео- дорожек (**MediaStreamTrack**). Можно достать все дорожки из MediaStream, вызвав команду `MediaStream.getTracks()`, которая возвращает массив объектов из **MediaStreamTrack**.

## MediaStreamTrack

MediaStreamTrack обладает свойством **kind** (audio или video, указывающий тип мультимедиа, который он воспроизводит). Каждую дорожку можно выключить, переключив ее свойство **enabled**. У дорожки есть логическое свойство remote, которое показывает, является ли она источником `RTCPeerConnection` и идет ли она от удаленного узла.