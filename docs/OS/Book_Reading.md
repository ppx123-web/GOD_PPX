# OSTEP READING

## 条件变量

生产者和消费者问题： OSTEP第291页，注意到一个条件变量是否够用，因为pthread_cond_signal唤醒一个或多个等待的线程，而不是全部。

所以关于唤醒，pthread_cond_broadcast 可以保证正确性，但是当线程数比较多时影响性能。

使用signal要十分注意，很有可能死锁。

## 信号量

信号量也要注意临界区的保护，还有不要死锁。

信号量的sem_wait(sem)即P，相当于consumer，不能取时在sem上睡眠，sem_post(sem)即V，相当于producer，当信号量的值大于0时，唤醒睡在sem上的线程。

## 互斥锁和自旋锁

互斥锁当锁已被获取时，其他想要获得锁的程序被挂起，直到锁被释放。
