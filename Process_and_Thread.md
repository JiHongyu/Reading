以下内容均摘自Internet

[面试总结-进程、线程与多线程](http://www.cnblogs.com/wuchanming/p/3992395.html?utm_source=tuicool&utm_medium=referral)

[linux 下 进程和线程的区别（baidu 面试)](http://blog.csdn.net/forrest2009/article/details/6413756)

进程: 程序执行时的一个实例，即它是程序已经执行到课中程度的数据结构的汇集。从内核的观点看，进程的目的就是担当分配系统资源（CPU时间、内存等）的基本单位。

线程: 进程的一个执行流，是CPU调度和分派的基本单位，它是比进程更小的能独立运行的基本单位。

>进程——资源分配的最小单位，线程——程序执行的最小单位


[可重入函数与线程安全函数概念介绍](http://waret.iteye.com/blog/744169)

[线程安全与可重入](http://www.cnblogs.com/welkinwalker/archive/2011/01/03/1925027.html)



Qt 多线程编程模型
```c++

 class MyThread : public QThread
 {
 public:
     void run();
 };

 void MyThread::run()
 {
     QTcpSocket socket;
     // connect QTcpSocket's signals somewhere meaningful
     ...
     socket.connectToHost(hostName, portNumber);
     exec();
 }

```

Python 多线程编程模型
```python

import threading
import math
class CThreadTest(threading.Thread):
    
    def __init__(self):
        threading.Thread.__init__(self)

    def run(self):
        i = 12.3
        while True:
            i = math.sin(i)*math.sin(i) + math.cos(2*i)

```

C++11 多线程
