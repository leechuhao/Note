## HashMap In Java 7

```
final int hash(Object k) {
   int h = hashSeed;
   if (0 != h && k instanceof String) {
       return sun.misc.Hashing.stringHash32((String) k);
   }
//扰动 防止最后几个值相同就发生冲突
   h ^= k.hashCode();
   h ^= (h >>> 20) ^ (h >>> 12);
   return h ^ (h >>> 7) ^ (h >>> 4);
}
//使用&替代了%运算，提高效率
static int indexFor(int h, int length) {
   return h & (length-1);
}
```

HashMap 的默认容量是16，负载因子 loadfactory 为0.75（通过数学计算可以得到当负载因子为log（2）=0.693时，为最佳数字，而且0.75=3/4与2的幂相乘为整数）

## HashTable In Java 7

上面是Java 7中HashMap的`hash`方法以及`indexOf`方法的实现，那么接下来我们要看下，线程安全的HashTable是如何实现的，和HashMap有何不同，并试着分析下不同的原因。以下是Java 7中HashTable的hash方法的实现。

```
private int hash(Object k) {   // hashSeed will be zero if alternative hashing is disabled.   return hashSeed ^ k.hashCode();}
```

我们可以发现，很简单，相当于只是对k做了个简单的hash，取了一下其hashCode。而HashTable中也没有`indexOf`方法，取而代之的是这段代码：

```
int index = (hash & 0x7FFFFFFF) % tab.length;
```

也就是说，HashMap和HashTable对于计算数组下标这件事，采用了两种方法。HashMap采用的是位运算，而HashTable采用的是直接取模。

> 为啥要把hash值和0x7FFFFFFF做一次按位与操作呢，主要是为了保证得到的index的的二进制的第一位为0（一个32位的有符号数和0x7FFFFFFF做按位与操作，其实就是在取绝对值。），也就是为了得到一个正数。因为有符号数第一位0代表正数，1代表负数。

HashTable默认的初始大小为11，之后每次扩充为原来的2n+1。也就是说，HashTable的链表数组的默认大小是一个素数、奇数。之后的每次扩充结果也都是奇数。

由于HashTable会尽量使用素数、奇数作为容量的大小。当哈希表的大小为素数时，简单的取模哈希的结果会更加均匀。（这个是可以证明出来的，由于不是本文重点，暂不详细介绍，可参考：http://zhaox.github.io/algorithm/2015/06/29/hash）

## ConcurrentHashMap In Java 7

```
private int hash(Object k) {   int h = hashSeed;   if ((0 != h) && (k instanceof String)) {       return sun.misc.Hashing.stringHash32((String) k);   }   h ^= k.hashCode();   // Spread bits to regularize both segment and index locations,   // using variant of single-word Wang/Jenkins hash.   h += (h <<  15) ^ 0xffffcd7d;   h ^= (h >>> 10);   h += (h <<   3);   h ^= (h >>>  6);   h += (h <<   2) + (h << 14);   return h ^ (h >>> 16);}int j = (hash >>> segmentShift) & segmentMask;
```

上面这段关于ConcurrentHashMap的hash实现其实和HashMap如出一辙。都是通过位运算代替取模，然后再对hashcode进行扰动。区别在于，ConcurrentHashMap 使用了一种变种的Wang/Jenkins 哈希算法，其主要目的也是为了把高位和低位组合在一起，避免发生冲突。至于为啥不和HashMap采用同样的算法进行扰动，我猜这只是程序员自由意志的选择吧。至少我目前没有办法证明哪个更优。

## HashMap In Java 8

在Java 8 之前，HashMap和其他基于map的类都是通过链地址法解决冲突，它们使用单向链表来存储相同索引值的元素。在最坏的情况下，这种方式会将HashMap的get方法的性能从`O(1)`降低到`O(n)`。

为了解决在频繁冲突时hashmap性能降低的问题，Java 8中使用平衡树来替代链表存储冲突的元素。这意味着我们可以将最坏情况下的性能从`O(n)`提高到`O(logn)`。关于HashMap在Java 8中的优化，我后面会有文章继续深入介绍。

如果恶意程序知道我们用的是Hash算法，则在纯链表情况下，它能够发送大量请求导致哈希碰撞，然后不停访问这些key导致HashMap忙于进行线性查找，最终陷入瘫痪，即形成了拒绝服务攻击（DoS）。

关于Java 8中的hash函数，原理和Java 7中基本类似。Java 8中这一步做了优化，只做一次16位右位移异或混合，而不是四次，但原理是不变的。

```
static final int hash(Object key) {   int h;   return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);}
```

在JDK1.8的实现中，优化了高位运算的算法，通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的。以上方法得到的int的hash值，然后再通过`h & (table.length -1)`来得到该对象在数据中保存的位置。

## HashTable In Java 8

在Java 8的HashTable中，已经不在有hash方法了。但是哈希的操作还是在的，比如在put方法中就有如下实现：

```
int hash = key.hashCode();int index = (hash & 0x7FFFFFFF) % tab.length;
```

这其实和Java 7中的实现几乎无差别，就不做过多的介绍了。

## ConcurrentHashMap In Java 8

Java 8 里面的求hash的方法从hash改为了spread。实现方式如下：

```
static final int spread(int h) {   return (h ^ (h >>> 16)) & HASH_BITS;}
```

Java 8的ConcurrentHashMap同样是通过Key的哈希值与数组长度取模确定该Key在数组中的索引。

不同的是，Java 8的ConcurrentHashMap作者认为引入红黑树后，即使哈希冲突比较严重，寻址效率也足够高，所以作者并未在哈希值的计算上做过多设计，只是将Key的hashCode值与其高16位作异或并保证最高位为0（从而保证最终结果为正整数）。