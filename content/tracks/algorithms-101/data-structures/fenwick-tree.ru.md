---
title: Дерево Фенвика
description: Дерево Фенвика
authors:
categories: ['programming', 'Data Structures', 'Дерево отрезков', 'Дерево Фенвика']
tags: ['Data Structures']
# series: null
# featuredImage: null
toc: false
weight: 10
date: 2023-02-10
lastmod: 2023-02-10
published: true
prerequisites:
  - Бинарное дерево
  - Префиксные суммы
---

Дерево Фенвика, также известное как двоичное индексированное дерево (Binary Indexed Tree, BIT).

Терминология:

    a - исходный массив
    tree - массив дерева, полученный в результате преобразований массива 'a'
    i - индекс массива
    k - индекс массива
    F(i) - еще не определенный индекс, полученный в реузльтате преобразования индекса `i`. F(i) <= i
        F(i) - функция, которую создадим позже.

```
   0  1  2  3  4  5  6  7       
a[ 5, 7, 9, 3, 8, 2, 4, 6]      
```

1. Сумма по текущему индексу содержит данные **только с предыдущих** индексов, **не обязательно с нулевого**.
2. В каждой ячейке реузльтируещего массива хранится сумма ячеек отрезка `k`. `k`- не константное число и может меняться. 

    Т.е. в одной ячейке может храниться сумма за предудущий отрезок длинною 2 символа, а в другой ячейке за отрезок длиною 5 символов. 

![fenwick-tree.ru.jpg](../assets/fenwick-tree.ru.jpg)
![fenwick-tree-prefix-sum-compare.ru.png](../assets/fenwick-tree-prefix-sum-compare.ru.png)

**По вертикали — индексы массива `a`**

**По горизонтали — индексы массива `tree`**

**($T_i$ является суммой элементов массива `a`, индексы которых заштрихованы), по вертикали — индексы массива `a`**

В данном примере в `tree[1]` хранится сумма элементов на отрезве `a[0:2]` . **2 - потому что последний элемент по индексу не включается**.

    tree[9] = sum(a[8:10])
    tree[7] = sum(a[0:8])

----

На данном этапе получаем структуру дерева вида: `tree[i] = sum(a[F(i):i+1])`.  Примечание: `i+1` - чтобы *захватить* последний элемент по индексу.

Если мы рассчитаем такое `F(i)` и умеем считать [сумму на префиксе](../prefix-sum), то сможем находить сумму любого подотрезка за `O(logn)`.

> [Как формируется массив дерева | youtube](https://youtu.be/BzFN9YwR-NM?t=1830)

Подсчет префиксных сумм:

```python
    def prefix_sum(k):
        result = 0
        i = k
        while i >= 0:
            result += tree[i]
            i = F(i) - 1
```

## Ресурсы

- https://www.youtube.com/watch?v=BzFN9YwR-NM
- [Дерево Фенвика | wiki](https://neerc.ifmo.ru/wiki/index.php?title=%D0%94%D0%B5%D1%80%D0%B5%D0%B2%D0%BE_%D0%A4%D0%B5%D0%BD%D0%B2%D0%B8%D0%BA%D0%B0)