### String

String 的底层实现是一个final char[] ，是不可修改的。每次对String 进行赋值都是在常量池新建一个对象，然后修改String 的引用。

```
/** The value is used for character storage. */
    private final char value[];
```

### AbstractStringBuilder

内部是一个char[] 对象以及一个容量count。

### StringBuffer

继承AbstractStringBuilder， 源码内几乎所有的方法都添加了synchronized 关键字

因此Stringbuffer是线程安全的，但同时synchronzied 关键字也对StringBuffer的性能产生了影响

### StringBuilder

同样是继承了 AbstractStringBuilder，与Stringbuffer最大的区别就是没有使用synchronized 关键字不能保证线程安全。

### 总结：

经过了jdk的不断地更新，Stringbuffer和StringBuilder的性能区别不是很大（主要是synchronized 关键字的优化），平时还是使用StringBuilder比较多。



### 