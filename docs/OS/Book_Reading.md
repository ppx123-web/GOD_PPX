# OSTEP READING

## 条件变量

生产者和消费者问题： OSTEP第291页，注意到一个条件变量是否够用，因为pthread_cond_signal唤醒一个或多个等待的线程，而不是全部。

所以关于唤醒，pthread_cond_broadcast 可以保证正确性，但是当线程数比较多时影响性能。

## 信号量

信号量也要注意临界区的保护，还有不要死锁。