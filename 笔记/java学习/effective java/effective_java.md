## 第一章：创建和销毁对象

### 1、使用静态工厂方法代替构造方法

- 工厂方法名称可以更好了解返回的对象
- 每次调用不必都创建新的对象
- 可以返回任何继承了改对象的所有子对象（在方法中进行判断，按需求返回）
- 在创建参数化类型对象的时候更简洁

```
Map<String , List<String>> map = new HashMapString , List<String>>();

public static <K, V> HashMap<K, V> newInstance() {
        return new HashMap<>();
    }
Map<String, List<String>> map = newInstance();
```

### 2、多个属性推荐使用builder模式

首先，创建对象的方法通常是使用构造方法以及 getter/setter，当你使用多个参数的构造方法是显得非常复杂而且难以维护，而getter/setter方法把对象的属性分成不同的语句进行赋值，尤其是那些必要的属性，在多线程的时候可能会出现问题（其实平时没有多线程的时候没有觉得很麻烦）

使用builer模式就显得很专业（划掉），代码易读，对于可变参数来说赋值非常方便，可以在builder() 方法中为某些属性自动赋值。

所以在存在多个属性的时候builder模式比较适用，推荐使用 lombok 的@Builder 注解来实现；

### 3、用私有构造器和枚举强化单例模式

```
public class Demo{
	//public static Demo INSTANCE = new Demo();
	//private Demo(){}
	
	private static Demo INSTANCE = new Demo();
	private Demo(){}
	public static Demo getInstace(){
		return INSTANCE
	}	
}

public enum Demo{
	INSTANCE;
}
```

### 4、使用私有构造器强化不可实例化的能力

当希望本对象不被外部创建的时候，应该避免出现缺省的空构造函数。（使用私有构造器应该加上注释）

### 5、避免创建不必要的实例

当实例会被重复使用，就不应该浪费太多时间去创建。如果有些时候重用对象代价过大，就去创建新的，别太执着对象重用。

这里引申一点，延迟初始化可能会导致代码更复杂

### 6、消除过期的对象引用

在一些自己管理内存的类中（比如Stack），如果没有显式清除引用，就不会被垃圾回收器自动回收，这样就会造成内存泄漏（但不要把显式清空对象引用作为规范，应该是作为例外，最好的办法是结束对象的生命周期）

造成内存泄漏的主要原因有：

1. 自己管理内存的类
2. 缓存
3. 监听器以及其他回调

### 7、不要使用终结方法

一定不要使用终结方法，使用终结方法确实可以增加对象回收的几率，但仍然无法确定对象什么时候会回收，甚至对象不会回收。

而且终结方法会严重的影响性能

唯一例外的是io类，这些类都有显式的终结方法

## 第二章：所有对象都通用的方法

### 8、覆盖equals方法遵守通用约定

不进行覆盖：

1. 类的每一个实例都是唯一的（Thread, enum）
2. 不关心类是否提供了“逻辑相等”的测试功能
3. 超类已经覆盖了equals方法
4. 类是私有的或者是包级别私有的，可以确定equals方法不会被调用的

覆盖方法的原则：

- 自反性
- 对称性
- 传递性
- 一致性
- 非空性：null

实现高质量equals方法的诀窍：

1. 使用==操作符检查“参数是否为这个对象的引用”
2. 使用 instanceof 操作符检查“参数是否为正确的类型”
3. 把参数转换成正确的类型
4. 比较的时候先比较那些容易检查出来的域（关键域）

### 9、覆盖equals方法时记得覆盖hashCode方法

如果没有覆盖hashCode方法，会导致该类无法与机遇散列的集合一起使用（HashMap、HashSet、Hashtable）

### 10、覆盖toString 方法

### 11、谨慎地覆盖clone

在覆盖clone方法的时候需要注意几点：

- 实现Cloneable 接口
- 当一个类覆盖了 clone的时候，需要留意其父类是否也实现了 clone

关于clone的约定内容：

```
x.clone() != x
x.clone().getClass() == x.getClass()
x.clone().equals(x)
```

- 覆盖clone前需要考虑类中是否有对象，clone实现的是浅复制还是深复制

可以使用工厂模式，提供clone功能，并且更具灵活性

### 12、考虑实现Comparable接口

compareTo 方法是Comparable接口的唯一方法，实现了接口则表明实例具有内在的排序关系。

通用约定：将这个对象与指定的对象进行比较。当该对象小于、大于、等于指定对象。分别返回负整数、正整数、零。

在跨越不同类的时候，compareTo可以不进行活动。

## 第三章：类和接口

### 13、使类和成员的可访问性最小化

简单来说就是**封装**，可以令代码模块化。Java的访问控制机制决定了类、接口、成员的可访问性。

序列化有可能令私有的成员“泄漏”

最常用的方法是：将类的成员私有化，再提供访问的API

### 14、在公有类中使用访问方法而非公有域

将类的成员私有化，再提供访问的API

例外：私有类、私有的嵌套类直接暴露成员没有太大影响

### 15、使可变性最小化

不可变类的规则：

- 不要提供修改对象状态的方法（setter）
- 类不被拓展（继承）
- 所有域都是final
- 所有域都是 private
- 类里面如果有可变对象，就不允许以任何方式访问或者调用

例子：String、基本类型包装类

### 16、复合优先于继承

继承违背了封装原则，只有当子类和超类之间存在子类型关系时使用继承。可以用复合和转发机制来代替继承（decorator模式，包装类）

### 17、为了继承设计类，必须加上说明

文档说明描述的是方法做了什么事情，而不是怎么去做。方法的具体实现可能会随着版本迭代而修改。

应该尽可能少地暴露受保护的成员，因为每个方法或者域都代表了一个关于实现细节的承诺。另一方面，又不能暴露太少，漏掉的受保护方法会导致类无法被真正的继承。

为了继承设计的类，唯一的测试方法是编写子类

构造器方法不能调用覆盖的方法

### 18、接口优于对象

设计共有接口必须非常谨慎，接口一旦发行，想再改变这个接口几乎不可能。因此在接口不可变之前尽可能多的实现，从而在发布之前找出漏洞。

当演变的容易性比灵活性和功能更为重要的时候，应该使用抽象类来定义类型。

### 19、接口只用于定义类型

常量接口是指用接口存放常量，所有子类都会收到影响。应该使用不可实例化的工具类来导出常量。

接口应该只被用来定义类型，而不应该用来导出常量

### 20、类层次优于标签类

标签类是指一个类为了能表示多种对象而而实现这些对象的具体方法。

标签类很少有用到的时候。当你想写一个标签类时，考虑一下是否能取消这个标签，是否可以用类层次替代。

### 21、函数对象表示策略

函数对象例子：compare（int a, int b）

在其他动态类型的语言中这个方式更加突出，比如pyhton的函数式编程，还有java8里新增加的特性，将方法本身作为参数对于编程确实变得灵活。

### 22、优先考虑静态成员类

嵌套类：静态成员类、非静态成员类、匿名类、局部类，后三者都被称为内部类

- 静态成员类
  - 静态成员类是外围类的一个静态成员。
  - 常见用法：作为共有的辅助类，比如计算器的各种操作
- 非静态成员类
  - 非静态成员类的每个实例都隐含这与外围类的一个外围实例相关联。在非静态成员类的实例方法内部，可以调用外围实例上的方法，或者用this获取外围实例的引用。非静态成员类不能脱离外围类单独使用。
  - 常见用法：定义一个adapter方法
- 匿名类
  - 只在被使用的时候同时被声明和实例化，当且仅当匿名类出现在非静态的环境中时，才会有外围实例；但是即使出现在静态环境中，也不能拥有任何静态成员。
  - 常用方法：动态创建函数对象，比如使用匿名的Comparator实例对字符串排序；创建过程对象，比如Runnable、Thread或者TimerTask实例；静态工厂方法的内部
- 局部类
  - 在任何可以声明局部变量的地方都可以声明局部类
  - 和成员类一样，有名字，可以重复使用
  - 和匿名类一样，只有当局部类是在非静态环境中定义的时候才会有外围实例，也不能包含静态成员，必须非常简短

## 第四章：泛型

### 23、不要在新代码中使用原生态类型

原生态类型是指没有使用泛型的原来类型比如 List和List\<E>，原生态类型无法在编译过程中检测类型，提前抛出异常。

由于泛型可以在运行时被擦除，所以会有两个例外情况。

- 在类文字（class）中必须使用原生态类型。如：List.class

- instanceof操作符，在参数化类型而非无限制通配符类型（List<?>）上使用instanceof是非法的

### 24、消除非受检警告

不要忽略非受检警告，每一条警告都表示有可能在运行时抛出ClassCastException异常。如果无法消除非受检警告，同时可以证明代码是类型安全的，就可以在尽可能小的范围里使用@SupperessWarnings("unchecked")注解禁止警告，同时要把原因记录下来。

### 25、列表优先于数组

无论是列表还是数组都不能将String放进Long容器里，但是利用数组会在运行时才发现错误，而利用列表会在编译时发现错误。

如果混合使用列表和数组并出现编译时错误或警告，第一选择应该是使用列表代替数组

### 26、优先考虑泛型

鼓励使用列表，实际上并不可能总是能在泛型中使用列表。

使用泛型比使用需要在代码中进行转换的类型来得更加安全，也更容易。在设计新类型的时候，要确保不需要进行这种转换就可以使用，这通常意味着要把类做成是泛型的。

### 27、优先考虑泛型方法

泛型方法的特性是无需明确指定类型参数的值。和泛型一样，应该确保新方法可以不用转换就能使用，这通常意味着要泛型化。而且还应该将现有的方法泛型化，使新用户使用起来更轻松，且不会破坏现有的客户端。

### 28、利用有限通配符来提升API的灵活性

为了获得最大限度的灵活性，要在表示生产者或者消费者的输入参数上使用通配符类型。（\<? extends E> or \<? super E>）

通配符使用原则 PECS：producer-extends, consumer-super.所有的comparable和comparator都是消费者。

### 29、优先考虑类型安全的异构容器

类型安全：当你请求String的时候不会返回Integer

异构容器：不像普通的map，所有键都是不同类型的。

集合api说明了泛型的一般用法（List\<String> 、Map\<Integer, String>），限制了每个容器只能有固定数目的**类型参数**，可以通过将类型参数放在键上而不是容器来绕过这个限制。对于这种类型安全的异构容器。可以用Class对象作为键，以这种形式使用的Class对象被称为**类型令牌**

## 第五章：枚举和注解

### 30、用enum代替int常量

不使用int常量主要原因是不能直观的知道这个常量表示的意义，而String常量又很容易出现错误，比如说拼写错误或者是性能问题。

我觉得最大的问题是复用，当你一个常量代表的一个意义重复被使用时，你会忘记这个常量原来的意思，不利于看代码和后续的修改（毕竟一个月前的代码自己也不认识了）

这里还提到枚举类型，枚举类型不止能设置常量，如果你的每种类型的行为都与常量相关，可以在枚举里定义抽象方法实现功能（**特定于常量的方法实现**）

```
public enum Operation{
    PLUS("+"){ double apply(double x, duoble y){return x+y;}}
    MINUS("-"){ double apply(double x, duoble y){return x-y;}}
    TIMES("*"){ double apply(double x, duoble y){return x*y;}}
    DIVIDE("/"){ double apply(double x, duoble y){return x/y;}}
   
   	private final String symbol;
   	Operation(String symbol){this.symbol = symbol;}
    abstract double apply(double x, double y);
}
```

枚举类型中的抽象方法必须被所有常量的具体方法覆盖，如果新添加的常量不写这个方法会报错

### 31.用实例域替代序数

枚举类型自带一个ordinal 序数，表示常量在枚举中的顺序，但是一定不要使用这个序数来使用枚举，这个数和具体的枚举类型没有任何联系，如果后续出现新的枚举或者改变位置，代码直接废了，全部要检查一遍。

### 32.用EnumSet代替位域

如果一个枚举类型主要用在集合中，一般会用int枚举模式，将2的不同倍数保存为常量

```
public class Text{
    public static final int STYLE_BOLD = 1<<0;
    public static final int STYLE_ITALIC = 1<<1;
    public static final int STYLE_UNDERLINE = 1<<2;
    public static final int STYLE_STRIKETHROUCH = 1<<3;
    
    public void applyStyles(int styles){ ... }
}
	//使用方法
	text.applyStyles(STYLE_BOLD | STYLE_STRIKETHROUCH);
```

这种表示法能用OR运算把几个常量合并到一个集合中，称为**位域**。

有个更方便的的EnumSet 类可以实现这个功能

```
public class Text{
    public enum Style{BOLD, ITALIC, UNDERLINE, STRIKETHROUCH }
    
    public void applyStyles(Set<Style> styles){ ... }
    //使用方法
    text.applyStyles(EnumSet.of(Style.BOLD, Style.STRIKETHROUCH));
}
```

EnumSet内部使用 单个long 来实现，性能比位域没有低太多，批处理都是利用位算法来实现，但是更安全。

总结一下：这些写的是人话吗？位域就是说将一些不重复的数据用位来记录，方便传递和保存(比如说： 000001001)，每个位表示一个数据。但是这样在遍历等一些复杂操作的时候比较难用，而EnumSet这个类帮你封装好，直接用就行。

吐槽：讲关于位操作的居然没有一个位的表示图出来，是不是看这个书的都是大神，直接脑补位运算的

### 33.用EnumMap代替序数索引

前面都说了不要用ordinal 去写代码，结果这里直接来个用ordinal 做key的map。这里就单纯的推荐使用EnumMap 类

```
public class Herb{
    public enum Type{ ANNUAL, PERENNIAL, BIENNIAL }
    private final String name;
    private final Type type;
    
    Herb(String name, Type type){
        this.name = name;
        this.type = type;
    }
}

Map<Herb.Type, Set<Herb>> herbsByType = new EnumMap<Herb.Type, Set<Herb>>(Herb.Type.class);
for(Herb.Type t : Herb.Type.values){
    herbsByType.put(t, new HashSet<Herb>());
}
for(Herb h: garden){
    herbsByType.get(h.type).add(h);
}
```

### 34.用接口模拟可伸缩的枚举