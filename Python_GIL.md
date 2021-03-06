## 1 Python GIL：  
  
  
GIL是一个互斥锁，它只允许同一时间一个线程获得CPU的控制权，即使是在多核CPU上跑多线程任务也是这样。所以它是一个臭名昭著的python特性。  
  
  
## 2 为什么Python要有GIL呢？  
  
是因为Python用管理内存的引用数量来实现内存管理，当指向该块内存空间的引用数量为0时，这一块内存空间释放。  
  
但是如果有两个线程同时对引用数量进行改变（出现race condition）的时候，引用数量需要被保护，就需要锁。  
  
但是如果对多个线程共享的变量每一个都挂上一个锁，就可能会出现死锁的情况。  
  
因此，python的设计是对python解释器挂上一个锁，所有要求获得python解释器的线程都需要开锁，但是这样也就让cpu密集型的程序失去了多线程的优势。  
  
这个情况还有其他的解决办法，就是java用的垃圾回收机制，但是垃圾回收机制势必让单线程的程序损失了performance，所以又需要引入提升表现的其他机制，比如JIT compiler。

## 3 为什么python选择了GIL呢？  

许多的python扩展包都是C写的，这些C程序整合到python特性中，需要一个线程安全的内存管理机制，也就是GIL。GIL易于实现而且对于单线程的程序来说性能很好，所以为早期的CPython开发者提供了便利。  
  
## 4 怎么解决？  
  
很简单，用多进程就好了嘛。每一个进程获得一个自己的python interpreter和内存空间，所以不存在上述问题啦。但是多进程本来就比多线程要笨重一点。
