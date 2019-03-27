1. ### Spring中IOC容器Bean的生命周期

   1. Bean实例化  
   2. 属性注入  
   3. BeanFactory   
   4. ApplicationContext  
   5. postProcessBeforeInitialization
   6. initializingBean接口（或者自定义接口） 
   7. postProcessAfterInitialization

   相关接口：

   ```
   Bean的生命周期 
   BeanNameAware:Bean可以获取在BeanFactory中的名称 
   BeanFactoryAware：Bean可以获取所在的BeanFactory对象 
   ApplicationContextAware：可以获取Bean所在的ApplicationContext 
   InitializingBean:Bean属性设置完成后的自定义操作接口：例如设置自定义的初始化方法init-method 
   DisposableBean：销毁Bean的相关资源
   ```

2. ### Spring中Bean的作用域

   1. singleton
   2. prototype
   3. request
   4. session
   5. global session

   ![img](../images/242025553_1552555606893_F700E1D9126F56CAD8981C82A6A243D0)

3. 