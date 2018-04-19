### 创建型模式


1. 工厂方法模式（Factory Method）
- 普通工厂模式
- 多个工厂方法模式：在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是提供多个工厂方法，分别创建对象。
- 静态工厂方法模式
  总体来说，工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。在以上的三种模式中，第一种如果传入的字符串有误，不能正确创建对象，第三种相对于第二种，不需要实例化工厂类，所以，大多数情况下，我们会选用第三种——静态工厂方法模式。

2. 抽象工厂模式（Abstract Factory）
  这个模式的好处就是，如果你现在想增加一个功能：发及时信息，则只需做一个实现类，实现Sender接口，同时做一个工厂类，实现Provider接口，就OK了，无需去改动现成的代码。这样做，拓展性较好！

3. 单例模式（Singleton）
  在Java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。这样的模式有几个好处：
- 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。
- 省去了new操作符，降低了系统内存的使用频率，减轻GC压力。
- 有些类如交易所的核心交易引擎，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。
```
    public class Singleton {
    	/* 私有构造方法，防止被实例化 */
    	private Singleton() {
    	}
    	/* 此处使用一个内部类来维护单例 */
    	private static class SingletonFactory {
    	private static Singleton instance = new Singleton();
    	}
    	/* 获取实例 */
    	public static Singleton getInstance() {
    	  return SingletonFactory.instance;
    	  }
    	  /* 如果该对象被用于序列化，可以保证对象在序列化前后保持一致 */
    	  public Object readResolve() {
    	  return getInstance();
    	  }
    }
```
4、建造者模式（Builder）
建造者模式则是将各种产品集中起来进行管理，用来创建复合对象，所谓复合对象就是指某个类具有不同的属性，其实建造者模式就是前面抽象工厂模式和最后的Test结合起来得到的。

5、原型模式（Prototype）
该模式的思想就是将一个对象作为原型，对其进行复制、克隆，产生一个和原对象类似的新对象。
```
	public class Prototype implements Cloneable, Serializable {
	private static final long serialVersionUID = 1L;
	private String string;
	private SerializableObject obj;
	
	/* 浅复制 */
	public Object clone() throws CloneNotSupportedException {
		Prototype proto = (Prototype) super.clone();
		return proto;
	}
	
	/* 深复制 */
	public Object deepClone() throws IOException, ClassNotFoundException {
	
		/* 写入当前对象的二进制流 */
		ByteArrayOutputStream bos = new ByteArrayOutputStream();
		ObjectOutputStream oos = new ObjectOutputStream(bos);
		oos.writeObject(this);
	
		/* 读出二进制流产生的新对象 */
		ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
		ObjectInputStream ois = new ObjectInputStream(bis);
		return ois.readObject();
	}
	
	public String getString() {
		return string;
	}
	
	public void setString(String string) {
		this.string = string;
	}
	
	public SerializableObject getObj() {
		return obj;
	}
	
	public void setObj(SerializableObject obj) {
		this.obj = obj;
	}

}

class SerializableObject implements Serializable {
	private static final long serialVersionUID = 1L;
}
```