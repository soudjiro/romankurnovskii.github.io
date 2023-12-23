---
title: Префиксные суммы
description: Префиксные суммы
authors:
categories: ['programming', 'Data Structures', 'Префиксные суммы']
tags: ['Data Structures']
# series: null
# featuredImage: null
toc: false
weight: 10
date: 2023-02-10
lastmod: 2023-02-10
published: true
---

1. Подсчет префиксных сумм

Сумма в текущей ячейке равна сумме в предыдущей + значение текущей.

```
   0  1  2  3  4  5  6  7       0  1   2   3   4   5   6   7
a[ 5, 7, 9, 3, 8, 2, 4, 6] => b[5, 12, 21, 24, 32, 34, 38, 44]

0    5              [5,]
1    5 + 7 = 12     [5,12,]
2    9 + 12 = 21    [5,12,21,]
3    3 + 21 = 24    [5,12,21,24,]
...
```

Чтобы посчитать сумму на любом отрезкe `[L, R]`, достаточно вычесть сумму с **предыдущего индекса** от `L'

L = 3, R = 5: [3, 8, 2] = 13

Алгоритм:

1. Находим сумму (значение) в правой части отрезка: `b[R] = 34`
2. Вычитаем из значения `b[R]` значение `b[L-1]`

В новом массиве:

```
b[L - 1] = b[2] = 21
b[R] = 34

34 - 21 = 13
```

## Ресурсы

- <https://www.youtube.com/watch?v=BzFN9YwR-NM&t=797s>