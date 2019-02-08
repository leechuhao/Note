##### @Configuration标注在类上，将该类作为xml文件中的<beans>

```
@Configuration
public class TestConfiguration{
    public TestConfiguration(){
        System.out.println("spring容器启动初始化");
    }
}
```

相当于

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" xmlns:jdbc="http://www.springframework.org/schema/jdbc"  
    xmlns:jee="http://www.springframework.org/schema/jee" xmlns:tx="http://www.springframework.org/schema/tx"
    xmlns:util="http://www.springframework.org/schema/util" xmlns:task="http://www.springframework.org/schema/task" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-4.0.xsd
        http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-4.0.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd
        http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-4.0.xsd" default-lazy-init="false">
</beans>
```

在主方法中

```
public class TestMain {
    public static void main(String[] args) {

        //@Configuration注解的spring容器加载方式，用AnnotationConfigApplicationContext替换ClassPathXmlApplicationContext
        ApplicationContext context = new AnnotationConfigApplicationContext(TestConfiguration.class);

        //如果加载spring-context.xml文件：
        //ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");
    }
}
```

----

##### @Bean

标注在方法上，相当<bean>，用于注册bean对象

```
public class TestBean {

    public void sayHello(){
        System.out.println("TestBean sayHello...");
    }

    public String toString(){
        return "username:"+this.username+",url:"+this.url+",password:"+this.password;
    }

    public void start(){
        System.out.println("TestBean 初始化。。。");
    }

    public void cleanUp(){
        System.out.println("TestBean 销毁。。。");
    }
}

@Configuration
public class TestConfiguration {
        public TestConfiguration(){
            System.out.println("spring容器启动初始化。。。");
        }

    //@Bean注解注册bean,同时可以指定初始化和销毁方法
    //@Bean(name="testNean",initMethod="start",destroyMethod="cleanUp")
    @Bean
    @Scope("prototype")
    public TestBean testBean() {
        return new TestBean();
    }
}
```

另外

1. @Bean注解在返回实例的方法上，如果没有使用@Bean来指定bean的名称，则默认与方法名相同。
2. @Bean注解默认作用域为单例singleton作用域，可通过@Scope(“prototype”)设置为原型作用域
3. @Bean的作用是注册bean对象，那么完全可以使用@Component、@Controller、@Service、@Ripository等注解注册bean，当然需要配置@ComponentScan注解进行自动扫描。

