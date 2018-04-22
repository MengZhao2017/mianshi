# mianshi

##总结最近面试的问题
Volatile的特征：

A、禁止指令重排（有例外） 
B、可见性

Volatile的内存语义：

当写一个volatile变量时，JMM会把线程对应的本地内存中的共享变量值刷新到主内存。
