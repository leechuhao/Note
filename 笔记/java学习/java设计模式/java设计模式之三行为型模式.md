行为型模式
分为三类：
一、父类和子类之间
13. 策略模式（strategy）
	定义一些算法，封装起来，并且这些算法可以互相替换。最典型的是计算器。
14. 模板方法模式（Template Method）
	一个抽象类中，有一个主方法（主要的方法），再定义1...n个方法，可以是抽象的，也可以是实际的方法，定义一个类，继承该抽象类，重写抽象方法，通过调用抽象类，实现对子类的调用.
	
二、两个类之间
15. 观察者模式（Observer）
	当一个对象变化时，其它依赖该对象的对象都会收到通知，并且随着变化！对象之间是一种一对多的关系。

16. 迭代子模式（Iterator）
	集合

17. 责任链模式（Chain of Responsibility）
	有多个对象，每个对象持有对下一个对象的引用，这样就会形成一条链，请求在这条链上传递，直到某一对象决定处理该请求。但是发出者并不清楚到底最终那个对象会处理该请求，所以，责任链模式可以实现，在隐瞒客户端的情况下，对系统进行动态的调整。（命令只允许由一个对象传给另一个对象，而不允许传给多个对象）

18. 命令模式（Command）
	命令模式的目的就是达到命令的发出者和执行者之间解耦，实现请求和执行分开

三、类的状态
19. 备忘录模式（Memento）
	有一个类中某个属性需要备份，创建了备份类和启动备份类。

20. 状态模式（State）
	可以通过改变状态来获得不同的行为；你的好友能同时看到你的变化。

四、通过中间类
21. 访问者模式（Visitor）
	访问者模式就是一种分离对象数据结构与行为的方法，通过这种分离，可达到为一个被访问者动态添加新的操作而无需做其它的修改的效果

22. 中介者模式（Mediator）
	中介类实现其它类并持有实例，解释说中介实现所有方法，其他类只要写好并放在中介。

23. 解释器模式（Interpreter）