### 核心代号

架构就是常看到的sandy bridge/ivy bridge/haswell/broadwell/skylake 都是架构的名字

### 命名

 i5、i7指的并不是一个具体产品，而是一个产品系列，它同样需要与代数挂钩，从命名上也能看出来，比如第一代i5通常是i5 750/i5 760，第二代是i5 2XXX，第三代i5 3XXX（第二代开始后面的数字第一位就代表第几代）而每一代都会更新架构，性能的提升也都来自于这里。而通常同一代的i3/i5/i7的架构是一样的。

台式机通常情况下：i3双核4线程，i5四核4线程，I7四核8线程

### 缓存

CPU又分为一级(L1)二级(L2)三级(L3)缓存，你通常会看到L1最小，L2次之，L3最大(很多普通CPU并没有三级，只有一二级)，成这种结构是因为，L1制造难度大，成本高，但往大了做对系统提升却比较有限，所以都很小。而CPU的读取顺序也是先从L1里读，然后L2→L3→内存。L2作为其外部缓冲，而L3就是L2的缓冲(备胎当到老)。缓存当然是越大越好，毕竟它们都比内存快嘛，但以目前相同情况下，L1还是越大越好，相同L1比L2，相同L2比L3。

### 字母

```
笔记本：
M 低电压
U 超低电压
台式：
K 可超频
X 至尊版
S 节能版
T 更节能版
R 核显强化版（看清楚功耗有没有降）
f 没有核显
```

### 总结

**核心代号>核心/线程>频率>缓存>制程>其他**

