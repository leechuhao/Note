| 注解        | 含义                                     |
| ----------- | ---------------------------------------- |
| @Component  | 最普通的组件，注入到spring容器中进行管理 |
| @Repository | 用于持久层                               |
| @Service    | 用于业务逻辑层                           |
| @Controller | 用于表现层                               |

##### @Component

`@Component`是一个通用的Spring容器管理的单例bean组件。而`@Repository`, `@Service`, `@Controller`就是针对不同的使用场景所采取的特定功能化的注解组件。因此，当你的一个类被`@Component`所注解，那么就意味着同样可以用`@Repository`, `@Service`, `@Controller`来替代它，同时这些注解会具备有更多的功能，而且功能各异。

----

`@Component`就是跟`<bean>`一样，可以托管到Spring容器进行管理。

`@Service`, `@Controller` , `@Repository` = {`@Component` + 一些特定的功能}。这个就意味着这些注解在部分功能上是一样的。

当然，下面三个注解被用于为我们的应用进行分层：

`@Controller`注解类进行前端请求的处理，转发，重定向。包括调用Service层的方法 
`@Service`注解类处理业务逻辑 
`@Repository`注解类作为DAO对象（数据访问对象，Data Access Objects），这些类可以直接对数据库进行操作 

----

##### 总结

`@Component`, `@Service`, `@Controller`, `@Repository`是spring注解，注解后可以被spring框架所扫描并注入到spring容器来进行管理 
`@Component`是通用注解，其他三个注解是这个注解的拓展，并且具有了特定的功能 
`@Repository`注解在持久层中，具有将数据库操作抛出的原生异常翻译转化为spring的持久层异常的功能。 
`@Controller`层是spring-mvc的注解，具有将请求进行转发，重定向的功能。 
`@Service`层是业务逻辑层注解，这个注解只是标注该类处于业务逻辑层。 