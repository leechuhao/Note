### CountDownLatch

维护一个countdown，当countdown变为0时执行主线程预设的代码

子线程使用ContDownLatch.counDown()减少countdown值

预设代码使用ContDownLatch.await()来监听countdown值



### CyclicBarrier

当N个子线程达到条件时，触发子线程中的预设代码（也可以触发主线程的任务）

执行预设代码的是子线程中的其中一个

子线程用CyclicBarrier.await()等待其他子线程

CyclicBarrier可以重用



### Semaphore

类似锁的概念，子线程通过Semaphore.acquire()来获得资源，然后Semaphore.release()释放资源