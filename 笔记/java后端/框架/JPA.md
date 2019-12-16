[教程](https://www.ibm.com/developerworks/cn/opensource/os-cn-spring-jpa/index.html)

### JPA和Hibernate的关系

JPA 是 hibernate 的一个抽象（就像JDBC和JDBC驱动的关系）：
- JPA 是规范：JPA 本质上就是一种  ORM 规范，不是ORM 框架 —— 因为 JPA 并未提供 ORM 实现，它只是制订了一些规范，提供了一些编程的 API 接口，但具体实现则由 ORM 厂商提供实现
- Hibernate 是实现：Hibernate 除了作为 ORM 框架之外，它也是一种 JPA 实现
  
从功能上来说， JPA 是 Hibernate 功能的一个子集

### JPA 包括 3方面的技术

- ORM  映射元数据：JPA 支持 XML 和  JDK 5.0 注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中。  
- JPA 的 API：用来操作实体对象，执行CRUD操作，框架在后台完成所有的事情，开发者从繁琐的 JDBC和 SQL代码中解脱出来。  
- 查询语言（JPQL）：这是持久化操作中很重要的一个方面，通过面向对象而非面向数据库的查询语言查询数据，避免程序和具体的  SQL 紧密耦合。

### JPA 基本注解

JPA 基本注解:@Entity, @Table, @Id, @GeneratedValue, @Column, @Basic，@Transient, @Temporal, 用 table 来生成主键详解

1. @Entity
       @Entity 标注用于实体类声明语句之前，**指出该Java 类为实体类，将映射到指定的数据库表**。如声明一个实体类 Customer，它将映射到数据库中的 customer 表上。
2. @Table
       当**实体类与其映射的数据库表名不同名**时需要使用 @Table 标注说明，该标注与 @Entity 标注并列使用，置于实体类声明语句之前，可写于单独语句行，也可与声明语句同行。
       @Table 标注的**常用选项是 name**，用于指明数据库的表名
       @Table标注还有一个两个选项 catalog 和 schema 用于设置表所属的数据库目录或模式，通常为数据库名。uniqueConstraints 选项用于设置约束条件，通常不须设置。
3. @Id
       @Id 标注用于声明一个实体类的属性映射为数据库的**主键列**。该属性通常置于属性声明语句之前，可与声明语句同行，也可写在单独行上。
       @Id标注也可置于属性的**getter方法之前**。
4. @GeneratedValue
       @GeneratedValue  用于标注**主键的生成策略**，通过 **strategy 属性指定**。默认情况下，JPA 自动选择一个最适合底层数据库的主键生成策略：SqlServer 对应 identity， MySQL 对应 auto increment。
   在 javax.persistence.GenerationType 中定义了以下几种可供选择的策略：
   - IDENTITY：采用数据库 ID自增长的方式来自增主键字段，Oracle 不支持这种方式；
   - AUTO： JPA自动选择合适的策略，是**默认选项**；
   - SEQUENCE：通过序列产生主键，通过 @SequenceGenerator 注解指定序列名，MySql 不支持这种方式
   - TABLE：通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于数据库移植。
5. @Basic
       @Basic 表示一个**简单的属性到数据库表的字段的映射**,**对于没有任何标注的 getXxxx() 方法,默认即为@Basic**
       fetch: 表示该属性的读取策略,有 EAGER 和 LAZY 两种,分别表示主支抓取和延迟加载,默认为 EAGER.
       **optional:表示该属性是否允许为null, 默认为true** 
6. @Column
       当实体的**属性与其映射的数据库表的列不同名**时需要使用@Column 标注说明，该属性通常置于实体的属性声明语句之前，还可与 @Id 标注一起使用。
       @Column 标注的常用属性是 name，用于设置映射数据库表的列名。此外，该标注还包含其它多个属性，如：unique 、nullable、length等。
       @Column 标注的 columnDefinition 属性: 表示该**字段在数据库中的实际类型**.通常 ORM 框架可以根据属性类型自动判断数据库中字段的类型,但是对于Date类型仍无法确定数据库中字段类型究竟是DATE,TIME还是TIMESTAMP.此外,**String的默认映射类型为VARCHAR**, 如果要将 String 类型映射到特定数据库的 BLOB 或TEXT 字段类型.
       @Column标注也可置于属性的getter方法之前
7. @Transient
       表示该**属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性.**
       如果一个属性并非数据库表的字段映射,就务必将其标示为@Transient,否则,ORM框架默认其注解为@Basic
8. @Temporal
       在核心的 Java API 中并没有定义 Date 类型的精度(temporal precision).  而在数据库中,表示 Date 类型的数据有 DATE, TIME, 和 TIMESTAMP 三种精度(即单纯的日期,时间,或者两者 兼备).**在进行属性映射时可使用@Temporal注解来调整精度.**
9. 用 table 来生成主键详解
       将**当前主键的值单独保存到一个数据库的表**中，主键的值每次都是从指定的表中查询来获得。这种方法生成主键的策略可以适用于任何数据库，不必担心不同数据库不兼容造成的问题。

```
/**
 * 注解@Entity映射的表名和类名一样
 * 注解@Table：当实体类与其映射的数据库表名不同名时使用。其中name，用于指明数据库的表名 
 */
@Table(name="JPA_CUTOMERS")
@Entity
public class Customer {
	private Integer id;
	private String lastName;
	private String email;
	private int age;
	private Date createdTime;
	private Date birth;
 
	public Customer() {}
 
//用 table 来生成主键详解（用的少）:
//将当前主键的值单独保存到一个数据库的表中，主键的值每次都是从指定的表中查询来获得
//这种方法生成主键的策略可以适用于任何数据库，不必担心不同数据库不兼容造成的问题
/*	@TableGenerator(name="ID_GENERATOR", --name 属性表示该主键生成的名称，它被引用在@GeneratedValue中设置的generator 值中(即这两个名字要一样）
			table="jpa_id_generators",   --table 属性表示表生成策略所持久化的表名
			pkColumnName="PK_NAME",      --pkColumnName 属性的值表示在持久化表中，该主键生成策略所对应键值的名称
			pkColumnValue="CUSTOMER_ID", --pkColumnValue 属性的值表示在持久化表中，该生成策略所对应的主键（跟pkColumnName属性可以确定唯一的一行，该行有很多列）
			valueColumnName="PK_VALUE",  --valueColumnName 属性的值表示在持久化表中，该主键当前所生成的值，它的值将会随着每次创建累加，（再加这个属性能确定唯一的那个点）
			allocationSize=100)          --allocationSize 表示每次主键值增加的大小, 默认值为 50
	@GeneratedValue(strategy=GenerationType.TABLE,generator="ID_GENERATOR")*/
	@GeneratedValue(strategy=GenerationType.AUTO)
	@Id
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
 
	/**
	 * 注解@Column 当实体的属性与其映射的数据库表的列不同名时需要使用
	 * nullable=false不能为空
	 */
	@Column(name="LAST_NAME",length=50,nullable=false)
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
 
	//如果 列名跟字段一样如列名是email，则可以不用写，相关加了@Basic，但字段的属性都是默认的
//	@Basic
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
 
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
 
	//---------------------------------------------------------------
	//注解@Temporal 调整时间精度 2015-11-11 10:11:11
	@Temporal(TemporalType.TIMESTAMP)
	public Date getCreatedTime() {
		return createdTime;
	}
	public void setCreatedTime(Date createdTime) {
		this.createdTime = createdTime;
	}
	
	//2015-11-11
	@Temporal(TemporalType.DATE)
	public Date getBirth() {
		return birth;
	}
	public void setBirth(Date birth) {
		this.birth = birth;
	}
 
	//工具方法. 不需要映射为数据表的一列. 如果没有加@Transient，则会出错，因为没有set方法
	@Transient
	public String getInfo(){
		return "lastName: " + lastName + ", email: " + email;
	}
 
}

```

下面总结一下使用 Spring Data JPA 进行持久层开发大致需要的三个步骤：

1. 声明持久层的接口，该接口继承 Repository，Repository 是一个标记型接口，它不包含任何方法，当然如果有需要，Spring Data 也提供了若干 Repository 子接口，其中定义了一些常用的增删改查，以及分页相关的方法。
2. 在接口中声明需要的业务方法。Spring Data 将根据给定的策略（具体策略稍后讲解）来为其生成实现代码。
3. 在 Spring 配置文件中增加一行声明，让 Spring 为声明的接口创建代理对象。配置了 <jpa:repositories> 后，Spring 初始化容器时将会扫描 base-package 指定的包目录及其子目录，为继承 Repository 或其子接口的接口创建代理对象，并将代理对象注册为 Spring Bean，业务层便可以通过 Spring 自动封装的特性来直接使用该对象。

