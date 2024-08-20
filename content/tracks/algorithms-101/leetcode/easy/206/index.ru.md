---
title: 206. Reverse Linked List
seoTitle: LeetCode 206. Reverse Linked List | Решение на Python.
description: LeetCode 206. Разворачиваем односвязный список. Разбор задачи.
toc: true
tags: [LinkedList, Easy]
categories: [Algorithms, Easy, LeetCodeTop75]
date: 2023-09-05
lastMod: 2023-09-05
featuredImage: https://picsum.photos/700/241?grayscale
weight: 206
---


[LeetCode задача 206](<https://leetcode.com/problems/reverse-linked-list/>)

## Задача

Дана голова односвязного списка. Задача состоит в том, чтобы развернуть этот список и вернуть его развернутую версию.

## Подсказки

В этой задаче нам нужно просто обойти односвязный список и изменить направление его указателей.

## Подход

Задача заключается в изменении направления указателей односвязного списка. Она может быть решена с помощью итеративного метода, при котором мы будем двигаться по списку, сохраняя предыдущий элемент, текущий и следующий. Затем мы просто изменим направление указателя `next` для текущего элемента, чтобы он указывал на предыдущий элемент.

Этот процесс можно визуализировать как переворачивание стрелок между узлами списка в обратном направлении. Изначально указатели направлены от головы списка к его концу. После разворота они будут направлены от конца к голове, превращая последний элемент в новую голову списка.

**Пример:**

Начальный список: [1 -> 2] должен стать [1 <- 2], а точнее [None <- 1 <- 2]

1. Имеем следующие значения:
   1. current = 1
   2. next = 2 (current.next)
   3. prev = None

2. Переставляем местами в определенном порядке:
   1. next = current.next (2)
   2. current.next -> None (prev)
   3. prev = current (1)
   4. current = next (2)
   5. После данных перестановок текущий указатель смотрит на 2, следующий по счету узел.

## Алгоритм

1. **Инициализация**: Инициализируем два указателя — один (`curr`) для текущего элемента и другой (`prev`) для предыдущего. Изначально `prev` будет `None`.

2. **Обход списка**: В цикле, пока текущий элемент не станет `None`, делаем следующее:
   1. Сохраняем указатель на следующий элемент (`next`).
   2. Изменяем указатель `next` текущего элемента, чтобы он указывал на `prev`.
   3. Перемещаем `prev` и `curr` на одну позицию вперед.

3. **Возврат результата**: В конце `prev` будет указывать на новую голову списка.

## Решение

```python
# Определение для односвязного списка.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

def reverseList(head):
    prev = None  # Инициализируем предыдущий элемент как None
    curr = head  # текущий

    while curr:
        next = curr.next  # Сохраняем следующий элемент
        curr.next = prev  # Меняем направление указателя
        prev = curr  # Перемещаем предыдущий элемент вперед
        curr = next  # Перемещаем текущий элемент вперед

    return prev
```