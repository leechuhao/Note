[mybatis部分文档](http://www.mybatis.org/mybatis-3/zh/index.html)

### Mybatis的导入

```
http://central.maven.org/maven2/org/mybatis/mybatis/3.4.6/mybatis-3.4.6.jar

<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

### 配置

```

<?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE configuration
         PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
 <configuration>
    <!--properties用于定义一些属性变量，以便在配置文件中调用-->
     <properties>
         <!--定义一个变量为driver的属性，在下面就可以用${driver}来获得其属性值-->
         <property name="driver" value="com.mysql.cj.jdbc.Driver"></property>
         <property name="url" value="jdbc:mysql://rm-wz9b714vle01fg8ijmo.mysql.rds.aliyuncs.com/change_hair"></property>
    </properties>
    
    <!--定义不同环境下的配置，便于区分生产、测试等环节的配置-->
    <environments default="development">
       <!--定义一个环境下的配置-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="root"/>
                <property name="password" value="Blackeye100"/>
            </dataSource>
        </environment>
    </environments>
    <!--用于设置mapper文件的引入-->
    <mappers>
        <!--resource方式引入mapper文件-->
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
</configuration>

```

### 常用的settings

```
<settings>
  #设置配置文件中的所有映射器已经配置的任何缓存，默认false。
  <setting name="cacheEnabled" value="true"/>
  #延迟加载的全局开关。当开启时，所有关联对象都会延迟加载，默认为false
  <setting name="lazyLoadingEnabled" value="true"/>
  #是否允许单一语句返回多结果集，默认为true
  <setting name="multipleResultSetsEnabled" value="true"/>
  #是否使用列标签代替列名，默认为true
  <setting name="useColumnLabel" value="true"/>
  #是否允许JDBC支持自动生成主键，默认为false
  <setting name="useGeneratedKeys" value="false"/>
  #指定 MyBatis 应如何自动映射列到字段或属性
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  #指定发现自动映射目标未知列（或者未知属性类型）的行为，默认NONE
  #NONE: 不做任何反应
  #WARNING: 输出提醒日志
  #FAILING: 映射失败 (抛出 SqlSessionException)
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  #配置默认的执行器。默认为SIMPLE
  #SIMPLE 就是普通的执行器；
  #REUSE 执行器会重用预处理语句； 
  #BATCH 执行器将重用语句并执行批量更新
  <setting name="defaultExecutorType" value="SIMPLE"/>
  #设置超时时间，它决定驱动等待数据库响应的秒数。 
  <setting name="defaultStatementTimeout" value="25"/>
  #为驱动的结果集获取数量（fetchSize）设置一个提示值  
  <setting name="defaultFetchSize" value="100"/>
  #是否允许在嵌套语句中使用分页。如果允许使用则设置为false。
  <setting name="safeRowBoundsEnabled" value="false"/>
  #是否开启自动驼峰命名规则（camel case）映射，默认为false
  <setting name="mapUnderscoreToCamelCase" value="false"/>
</settings>
```

### 工作过程

```
public static static void main(String[] args){
    try{
        String resource = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
        SqlSession s = sqlSessionFactory.openSession();
        
        UserMapper mapper = s.getMapper(UserMapper.class);
        
       User user = mapper,getUserByName("name");
       
       if(user != null){
           system.out.print(user.getname());
       }
    }catch(Exception e){
        e.printStackTrace();
    }
}
```



