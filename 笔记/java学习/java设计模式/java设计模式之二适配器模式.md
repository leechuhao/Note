6. 适配器模式（Adapter）
  适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配所造成的类的兼容性问题。
- 类的适配器模式
  在source类中重写接口的方法，在adapter中不必重写
- 对象的适配器模式
  在adapter类中持有source类的实例
- 接口的适配器模式
  接口中可以存在很多方法，但不是所有的方法都被需要。看可以用一个抽象类实现接口，即重写所有的方法，使用时直接调用抽象类中的方法（所以为什么要这个接口，直接写个类不可以吗？）

------

- 类的适配器模式：当希望将一个类转换成满足另一个新接口的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。
- 对象的适配器模式：当希望将一个对象转换成满足另一个新接口的对象时，可以创建一个Wrapper类，持有原类的一个实例，在Wrapper类的方法中，调用实例的方法就行。
- 接口的适配器模式：当不希望实现一个接口中所有的方法时，可以创建一个抽象类Wrapper，实现所有方法，我们写别的类的时候，继承抽象类即可。

7. 装饰模式（Decorator）
  装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例，
- 特点：动态添加、撤除新功能

8. 代理模式（Proxy）
  代理模式的应用场景：如果已有的方法在使用的时候需要对原有的方法进行改进，此时有两种办法：
  - 修改原有的方法来适应。这样违反了“对扩展开放，对修改关闭”的原则。
  - 就是采用一个代理类调用原有的方法，且对产生的结果进行控制。

9. 外观模式（Facade）
  外观模式就是将他们的关系放在一个Facade类中，降低了类类之间的耦合度，该模式中没有涉及到接口

10. 桥接模式（Bridge）
  桥接模式就是把事物和其具体实现分开，使他们可以各自独立的变化。桥接的用意是：将抽象化与实现化解耦，使得二者可以独立变化，像我们常用的JDBC桥DriverManager一样，JDBC进行连接数据库的时候，在各个数据库之间进行切换，基本不需要动太多的代码，甚至丝毫不用动，原因就是JDBC提供统一接口，每个数据库提供各自的实现，用一个叫做数据库驱动的程序来桥接就行了。

11. 组合模式（Composite）
   在处理类似树形结构的问题时比较方便
```
public class TreeNode {  
    private String name;  
    private TreeNode parent;  
    private Vector<TreeNode> children = new Vector<TreeNode>();  
      
    public TreeNode(String name){  
        this.name = name;  
    }  
    
    public String getName() {  
        return name;  
    }  
      
    public void setName(String name) {  
        this.name = name;  
    }  
      
    public TreeNode getParent() {  
        return parent;  
    }  
      
    public void setParent(TreeNode parent) {  
        this.parent = parent;  
    }  
      
    //添加孩子节点  
    public void add(TreeNode node){  
        children.add(node);  
    }  
      
    //删除孩子节点  
    public void remove(TreeNode node){  
        children.remove(node);  
    }  
      
    //取得孩子节点  
    public Enumeration<TreeNode> getChildren(){  
        return children.elements();  
    }  
}  
///////////////////////////////////////
public class Tree {  

    TreeNode root = null;  

    public Tree(String name) {  
        root = new TreeNode(name);  
    }  
      
    public static void main(String[] args) {  
        Tree tree = new Tree("A");  
        TreeNode nodeB = new TreeNode("B");  
        TreeNode nodeC = new TreeNode("C");  
          
        nodeB.add(nodeC);  
        tree.root.add(nodeB);  
        System.out.println("build the tree finished!");  
    }  
}  
```
12. 享元模式（Flyweight）
   享元模式的主要目的是实现对象的共享，即共享池，当系统中对象多的时候可以减少内存的开销，通常与工厂模式一起使用。
   适用于作共享的一些个对象，他们有一些共有的属性，就拿数据库连接池来说，url、driverClassName、username、password及dbname，这些属性对于每个连接来说都是一样的，所以就适合用享元模式来处理，建一个工厂类，将上述类似属性作为内部数据，其它的作为外部数据，在方法调用时，当做参数传进来，这样就节省了空间，减少了实例的数量。