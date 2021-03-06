1. ### Spring中的IOC容器初始化过程（BeanFactory初始化）

   - 从xml文件中读取数据，封装成resource对象
   - 从Resource对象读取文件编码（DTD等），转换为document
   - 通过Reader接口生成BeanDefinition
   - BeanDefinition使用BeanDefinitionRegistry完成容器注册

2. ### Spring中IOC容器Bean的生命周期

	1. Bean实例化  
	2. 属性注入  
	3. BeanFactory   
	4. ApplicationContext  
	5. postProcessBeforeInitialization（自定义注册前行为）
	6. initializingBean接口（或者自定义接口） 
	7. postProcessAfterInitialization（自定义注册后行为）

相关接口：

```
Bean的生命周期 
BeanNameAware:Bean可以获取在BeanFactory中的名称 
BeanFactoryAware：Bean可以获取所在的BeanFactory对象 
ApplicationContextAware：可以获取Bean所在的ApplicationContext 
InitializingBean:Bean属性设置完成后的自定义操作接口：例如设置自定义的初始化方法init-method 
DisposableBean：销毁Bean的相关资源
```

3. ### Spring中Bean的作用域

   1. singleton
   2. prototype
   3. request
   4. session
   5. global session

   ![img](../images/242025553_1552555606893_F700E1D9126F56CAD8981C82A6A243D0)

4. ### 什么是IOC

	​	IOC指的是控制反转，为什么叫反转呢？在编程中“正传”表示的是当一个对象A对另一个对象B产生依赖关系的时候需要这个对象A主动创建、管理这个具有依赖关系的对象B，这样写出来的程序耦合程度非常高，而且会非常凌乱。用了IOC之后，当对象A需要依赖另一个对象B时，会通知IOC容器，容器会生产出所需要的对象B给它，这个把B对象给对象A的过程称为依赖注入（DI），这个生产对象的过程使用到了工厂模式（BeanFactory）。

5. ### 属性注入有哪些形式

   - @autoeweied
     - 构造函数
     - 字段
     - 参数
   - @Resource(name="")
   - 构造方法
   - setter
   - 静态工厂
   - 实例工厂

6. ### ApplicationContext

继承了BeanFactory