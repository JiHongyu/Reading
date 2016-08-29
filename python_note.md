## Python 面试资源

[Python 面试问答 Top 25](http://python.jobbole.com/85873/ "Python 面试问答 Top 25")

[Python工程师面试题集合](http://python.jobbole.com/84153/ "Python工程师面试题集合")

[很全的 Python 面试题](http://python.jobbole.com/85231/ "很全的 Python 面试题")



## 异常
[Python 异常编程技巧](http://python.jobbole.com/85942/)

## 装饰器

装饰器服从 **封闭-开放原则**，让一个完整的逻辑执行实体添加额外的功能。其基本基本形式如下

```python
def Decorater(func):

    def inner():
        # Do something
        return func() # 注意内层函数返回调用的函数值，而不是函数对象

    return inner

@Decorater
def Foo():
    pass
```

`@` 为 Python 语法糖，其等价形式为 `Foo = Decorater(Foo)`

被修饰函数带有参数时的装饰器
```python
def Decorater(func):
    def inner(*args, **kwargs):
        # Do something
        return func(*args, **kwargs)
    return inner
```


上述的装饰器虽然已经完成了其应有的功能，即：装饰器内的函数代指了原函数，注意其只是代指而非相等，原函数的元信息没有被赋值到装饰器函数内部。例如：函数的注释信息。

如果使用 `@functools.wraps` 装饰装饰器内的函数，那么就会代指元信息和函数。

```python
import functools

def Decorater(func):
    @functools.wraps(func)
    def inner(*args, **kwargs):
        # Do something
        return func(*args, **kwargs)
    return inner

@Decorater
def Foo():
    ''' Hello '''
    pass
```

带有参数的装饰器，需要多加一层闭包函数
```python
def Decorator_ARG(*args, **kwargs):

    def Decorator(func):
        print(args)
        print(kwargs)

        def inner(*args, **kwargs):
            print(args)
            print(kwargs)
            return func()

        return inner

    return Decorator


@Decorator_ARG(a =1, b=2)
def Foo(*args, **kwargs):
    pass
```

可以理解为 `Foo = Decorator_ARG(a=1,b=2)(*args, **kwargs)`

## 生成器

生成器是通过一个或多个yield表达式构成的函数，每一个生成器都是一个迭代器（但是迭代器不一定是生成器）。

如果一个函数包含yield关键字，这个函数就会变为一个生成器。

生成器并不会一次返回所有结果，而是每次遇到yield关键字后返回相应结果，并保留函数当前的运行状态，等待下一次的调用。

由于生成器也是一个迭代器，那么它就应该支持next方法来获取下一个值。

生成器是迭代器，但你只能遍历它一次(iterate over them once)
因为生成器并没有将所有值放入内存中，而是实时地生成这些值。

Python 生成器可以理解为 C++ 函数对象的一种应用而已，因为它的里面保存了状态。

生成器 send 应用
```python
def func():
    for x in range(4):
        m = yield x
        print(m)
```


## 闭包

[基本概念](http://python.jobbole.com/83969/)

函数的抽象？

应用场景：

+ 装饰器
+ Partitial
+ lambda 表达式

## 并行综述：线程，进程和协程

[线程，进程和协程](http://www.kuqin.com/shuoit/20160805/352714.html)


线程模型

```python
import threading

def Foo():
    pass

# 函数风格
t1 = threading.Thread(target=Foo)
t1.start()

class Coo(threading.Thread):
    def __init__(self):
        super().__init__()

    def run(self):
        Foo()

# 类风格
t2 = Coo()
t2.start()
```

Python 线程控制机制（其他大多数语言相似）：

+ Lock 普通锁（不可嵌套）
+ RLock 普通锁（可嵌套）常用
+ Semaphore 信号量
+ event 事件
+ condition 条件

**Lock 或者 RLock**

```python
import threading

threading.aquire()
# 临界区
threading.release()
```

**Semaphore 信号量**

```python
import threading

# 最多允许4个线程同时运行
semaphore = threading.BoundedSemaphore(5)

def Foo():
    semaphore.acquire()
    # get resources
    semaphore.release()

for x in range(20):
    t = threading.Thread(target=Foo)
    t.start()

```

Event 和 Condition 略过

Python 的 queue 模块包含有 **线程安全** 的队列，可以很方便的实现 ** 生产-消费者模型 **

```python
import queue
import threading

def worker():
    while True:
        item = q.get()
        if item is None:
            break
        do_work(item)
        q.task_done()

q = queue.Queue()
threads = []
for i in range(num_worker_threads):
    t = threading.Thread(target=worker)
    t.start()
    threads.append(t)

for item in source():
    q.put(item)

# block until all tasks are done
q.join()

# stop workers
for i in range(num_worker_threads):
    q.put(None)
for t in threads:
    t.join()
```

[线程池学习](http://chrisarndt.de/projects/threadpool/api/)

进程太难，略过

[协程资料](http://www.cnblogs.com/chgaowei/archive/2012/06/21/2557175.html)

[资料2](http://my.oschina.net/tenking/blog/29521)


## MRO 机制

[资料](http://www.kuqin.com/shuoit/20160523/352147.html)