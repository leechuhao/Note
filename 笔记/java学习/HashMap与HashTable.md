##### 简单的回答：

1. HashMap是非线程安全的，HashTable是线程安全的。
2. HashMap的键和值都允许有null值存在，而HashTable则不行。
3. 因为线程安全的问题，HashMap效率比HashTable的要高。

### ConcurrentHashMap

ConcurrentHashMap基于concurrentLevel划分出了多个Segment来对key-value进行存储，从而避免每次锁定整个数组，在默认的情况下，允许16个线程并发无阻塞的操作集合对象，尽可能地减少并发时的阻塞现象。

HashMap中Value可以相同，但是键不可以相同