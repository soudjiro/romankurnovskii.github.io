---
title: Модуль потоков threading
description: Python 101
toc: true
authors:
tags:
categories:
series:
featuredImage:
date: "2022-06-28"
lastmod: "2022-06-28"
draft: false
weight: 205
---

Модуль `threading` в Python предоставляет возможность создавать и управлять потоками выполнения. Потоки - это легковесные процессы, которые выполняются параллельно в пределах одного процесса, что позволяет лучше использовать ресурсы компьютера.

Для создания нового потока необходимо создать объект `Thread` и передать в его конструктор функцию, которую вы хотите запустить в отдельном потоке. Затем вызовите метод `start()` у этого объекта, чтобы запустить поток. Если вы хотите дождаться завершения потока, вызовите метод `join()`, который блокирует текущий поток, пока поток, на который вы вызываете `join()`, не завершится.

Пример использования модуля `threading`:

```python
from time import sleep
import threading

def print_numbers():
    for i in range(10):
        sleep(1) # задержка печати для примера
        print(i)

def print_letters():
    for letter in ['a', 'b', 'c', 'd', 'e']:
        print(letter)

if __name__ == '__main__':
t1 = threading.Thread(target=print_numbers)
t2 = threading.Thread(target=print_letters)

t1.start()
t2.start()
print("Done!")
    t1.join()
    t2.join()

    print("Done!")
```

Здесь мы создали две функции `print_numbers()` и `print_letters()`, каждая из которых печатает набор символов в консоль. Затем мы создали два потока, один для каждой из этих функций, и запустили их, вызвав метод start(). Затем мы дождались завершения каждого потока, вызвав метод `join()`, и напечатали сообщение "Done!".

В последнем примере кода мы увидим, что каждый поток будет печатать свою информацию в консоль, в произвольном порядке, так как потоки будут конкурировать за доступ к ресурсу (в данном случае, к выводу в консоль). 

Результат может отличаться от запуска к запуску программы, так как порядок выполнения потоков не гарантирован и зависит от того, как ОС распределяет ресурсы между потоками.


Модуль `threading` также предоставляет другие полезные классы, такие как `Lock`, `Condition`, `Semaphore`, которые помогают управлять доступом к ресурсам между несколькими потоками.