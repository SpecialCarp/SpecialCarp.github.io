# java ssm

> Spring + SpringMVC +MyBatis

| 持久化层  | 编写持久化类(实体类)                | 分析模块所涉及的表(或对象)，<br/>确定表(或对象)之间的关系 |
| --------- | ----------------------------------- | --------------------------------------------------------- |
| Dao层     | 编写Dao接口和<br/>MyBatis的映射文件 | 根据系统需求，<br/>编写相应的接口方法及其执行SQL          |
| Service层 | 编写Service接口方法并实现           | 编写业务逻辑，调用Dao操作                                 |
| Web层     | 编写Controller类                    | 用于处理页面请求，<br/>并与业务逻辑层(Service层)交互      |
| Web层     | 编写jsp页面                         | 对业务数据进行呈现，<br/>并对用户的非法操作进行适当的控制 |

> 注：
>
> 1. 持久化层（pojo）的一个类一般对应数据库的一张表，类与类之间的关系，就是数据库表与表之间的关系，pojo类一般是数据的载体，承载数据库的字段信息
> 2. Dao层的方法只实现简单的数据交互（增删改查），不涉及复杂的逻辑
> 3. Service层调用了dao层里的数据交互方法，并将它们组合，形成一个较为复杂的逻辑，用来实现某种功能
> 4. Controller层并不编写任何的方法，它仅是判断用户请求信息，调用service层的业务逻辑方法，判断处理后的结果然后跳转页面，并将处理后的数据一起返回给客户端
> 5. jsp页面或者Html页面，将Controller层的返回的数据进行简单的处理，然后展示给用户

## 环境要求

- 操作系统：Windows

- Web服务器：Tomcat8.0

- Java环境：JDK8.0

- 开发工具：Eclipse

- 数据库：MySQL5.5

- 浏览器：谷歌

- 所需jar包：（35个）（无需一次性准备完）

  Spring框架（10个）

  - aopalliance-1.0.jar
  - aspectjweaver-1.8.10.jar
  - spring-aop-4.3.6.RELEASE.jar
  - spring-aspects-4.3.6.RELEASE.jar
  - spring-beans-4.3.6.RELEASE.jar
  - spring-context-4.3.6.RELEASE.jar
  - spring-core-4.3.6.RELEASE.jar
  - spring-expression-4.3.6.RELEASE.jar
  - spring-jdbc-4.3.6.RELEASE.jar
  - spring-tx-4.3.6.RELEASE.jar

  SpringMVC框架（2个）

  - spring-web-4.3.6.RELEASE.jar
  - spring-webmvc-4.3.6.RELEASE.jar

  MyBatis框架（13个）

  - ant-1.9.6.jar
  - ant-launcher-1.9.6.jar
  - asm-5.1.jar
  - cglib-3.2.4.jar
  - commons-logging-1.2.jar
  - javassist-3.21.0-GA.jar
  - log4j-1.2.17.jar
  - log4j-api-2.3.jar
  - log4j-core-2.3.jar
  - mybatis-3.4.2.jar
  - ogml-3.1.12.jar
  - slf4j-api.1.7.22.jar
  - slf4j-log4j12-1.7.22.jar

  MyBatis和Spring整合（1个）

  - mybatis-spring-1.3.1.jar

  数据库驱动（1个）

  - mysql-connector-java-5.1.40-bin.jar

  数据源dbcp（2个）

  - commons-dbcp2-2.1.1.jar
  - commons-pool2-2.1.1.jar

  JSTL标签库（2个）

  - taglibs-standard-impl-1.2.5.jar
  - taglibs-standard-spec-1.2.5.jar

  Jackson框架（3个）

  - jackson-annotations-2.8.6.jar
  - jackson-core-2.8.6.jar
  - jackson-databind-2.8.6.jar

  Java工具类（1个）

  - commons-lang3-3.4.jar

## MyBatis

> 它是java持久化框架之一，也是一种ORM(Object/Rwlational Mappig，关系对象映射)框架。其性能优异，高度的灵活性、可优化性和易于维护

- 支持普通SQL查询、储存过程以及高级映射的持久层框架
- 消除了几乎所有的JDBC代码和参数的手动设置以及对结果集的检索，并使用简单的XML或是注解进行配置和原始映射
- 将接口和java的pojo(Plain Old Java Object，普通Java对象)映射成数据库中的记录，使得Java开发人员可以使用面向对象的编程思想来操作数据库

### MyBatis的工作原理



![326517643](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/326517643.png)

详细讲解：

1. 读取MyBatis的配置文件mybatis-config.xml。mybatis-config.xml作为MyBatis的全局配置文件，配置了MyBatis的运行环境信息，其中主要内容是获取数据库连接。
2. 加载映射文件Mapper.xml。映射文件即SQL映射文件，该文件中配置了操作数据库的SQL语句，需要在MyBatis配置文件mybatis-config.xml中加载才能执行。mybatis-config.xml 文件可以加载多个映射文件，每个映射文件对应数据库中的一张表。
3. 构造会话工厂。通过MyBatis的环境配置信息构建会话工厂SqlSessionFactory。
4. 创建SqlSession会话对象。由会话工厂创建SqlSession对象，该对象中包含了执行SQL语句的所有方法。
5. Executor执行器。MyBatis底层定义了一个Executor接口来操作数据库，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护。
6. MappedStatement对象。在Executor接口的执行方法中有一个MappedStatement类型的参数，该参数是对映射信息的封装，用于存储要映射的SQL语句的id、参数等信息。Mapper.xml文件中一个SQL对应一个MappedStatement对象，SQL的id即是MappedStatement的id。
7. 输入参数映射。在执行方法时，MappedStatement对象会对用户执行SQL语句的输入参数进行定义（可以定义为Map、List类型、基本类型和pojo类型），Executor执行器会通过MappedStatement对象在执行SQL前，将输入的Java对象映射到SQL语句中。这里对输入参数的映射过程类似于JDBC编程中对preparedStatement对象设置参数的过程。
8. 输出结果映射。在数据库中执行完SQL语句后，MappedStatement对象会对SQL执行输出的结果进行定义（可以定义为Map、List类型、基本类型和pojo类型），Executor执行器会通过MappedStatement对象在执行SQL语句后，将输出结果映射至Java对象中。这种输出结果映射过程类似于JDBC编程对结果集的解析处理过程。

> 注：这个过程在学习前记住，在学习中对照，在学习后回过头来再看一遍

### 初始MyBatis

**MyBatis入门小程序**

> 模拟场景：
>
> ​	根据id查询客户信息

准备：

1. 数据库：

   [systop_crm.sql](https://pan.baidu.com/s/1MV4Dhq1QaQgRRldmbPL4dQ "提取码：carp")

2. 导入jar

![1604116077999](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604116077999.png)

- pojo类 com.systop.pojo.Customer.java

  ```java
  package com.systop.pojo;
  import java.util.Date;
  /**
   * 客户持久化类
   */
  public class Customer {
  	private Integer cust_id;		// 客户编号
  	private String cust_name;		// 客户名称
  	private Integer cust_user_id;	// 负责人id
  	private Integer cust_create_id;	// 创建人id
  	private String cust_source;		// 客户信息来源
  	private String cust_industry;	// 客户所属行业
  	private String cust_level;		// 客户级别
  	private String cust_linkman;	// 联系人
  	private String cust_phone;		// 固定电话
  	private String cust_mobile;		// 移动电话
  	private String cust_zipcode;	// 邮政编码
  	private String cust_address;	// 联系地址
  	private Date cust_createtime;	// 创建时间	
  	
  	//查看数据
  	@Override
  	public String toString() {
  		return "Customer [cust_id=" + cust_id + ", cust_name=" + cust_name + ", cust_user_id=" + cust_user_id
  				+ ", cust_create_id=" + cust_create_id + ", cust_source=" + cust_source + ", cust_industry="
  				+ cust_industry + ", cust_level=" + cust_level + ", cust_linkman=" + cust_linkman + ", cust_phone="
  				+ cust_phone + ", cust_mobile=" + cust_mobile + ", cust_zipcode=" + cust_zipcode + ", cust_address="
  				+ cust_address + ", cust_createtime=" + cust_createtime + "]";
  	}
  
  	public Customer() {
  	}
  	
  	// getter和setter方法
      // ...
  }
  ```

- 日志配置文件 log4j.properties

  ```properties
  # Global logging configuration
  log4j.rootLogger=ERROR, stdout
  # MyBatis logging configuration...
  log4j.logger.com.systop=DEBUG
  # Console output...
  log4j.appender.stdout=org.apache.log4j.ConsoleAppender
  log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
  log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
  ```

- sql映射文件 com.systop.mapper.CustomerMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
  <!-- namespace 命名空间：全路径 -->
  <mapper namespace="com.systop.mapper.CustomerMapper">
  	<!-- 根据id查询客户信息 id必须写而且不能重复 -->
  	<select id="findCustomerById" parameterType="Integer" resultType="com.systop.pojo.Customer">
  		select * from customer where cust_id = #{id}
  	</select>
  </mapper>
  ```

- 全局配置文件mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  	"http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  	<!-- 配置环境信息，默认使用mysql数据源 -->
  	<environments default="mysql">
  		<!-- 定义了一个id为mysql的数据源信息 -->
  		<environment id="mysql">
  			<!-- 使用事务管理JDBC -->
  			<transactionManager type="JDBC" />
  			<!-- 数据库连接池 -->
  			<dataSource type="POOLED">
  				<property name="driver" value="com.mysql.jdbc.Driver"/>
  				<property name="url" value="jdbc:mysql://localhost:3306/systop_crm"/>
  				<property name="username" value="root"/>
  				<property name="password" value="root"/>
  			</dataSource>
  		</environment>
  	</environments>
      <!-- 导入映射文件 -->
  	<mappers>
  		<mapper resource="com/systop/mapper/CustomerMapper.xml"/>
  	</mappers>
  </configuration>
  ```

- 编写运行类测试结果MybatisTest.java

  ```java
  package com.systop.test;
  
  import java.io.IOException;
  import java.io.InputStream;
  
  import org.apache.ibatis.io.Resources;
  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.apache.ibatis.session.SqlSessionFactoryBuilder;
  
  import com.systop.pojo.Customer;
  
  public class MybatisTest {
  
  	public static void main(String[] args) throws IOException {
  		//1、读取mybatis-config.xml  加载了CustomerMapper.xml
  		InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
  		//2、构建SqlSessionFactory工厂
  		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  		//3、通过工厂创建出SqlSession对象
  		SqlSession sqlSession = sqlSessionFactory.openSession();
  		//4、执行sql语句
  		Customer cust = sqlSession.selectOne("com.systop.mapper.CustomerMapper.findCustomerById", 14);
  		System.out.println(cust);
  		//5、关闭
  		sqlSession.close();
  	}
  
  }
  ```

运行结果：

```
DEBUG [main] - ==>  Preparing: select * from customer where cust_id = ? 
DEBUG [main] - ==> Parameters: 14(Integer)
DEBUG [main] - <==      Total: 1
Customer [cust_id=14, cust_name=小张, cust_user_id=null, cust_create_id=1, cust_source=7, cust_industry=3, cust_level=23, cust_linkman=小雪, cust_phone=010-88888887, cust_mobile=13848399998, cust_zipcode=100096, cust_address=北京昌平区西三旗, cust_createtime=Fri Apr 08 16:32:01 CST 2016]
```

### Mybatis的核心配置

#### Mybatis的核心对象

> SqlSessionFactory：Mybatis中十分重要的对象，他是单个数据库映射关系经过编译后的内存镜像，主要作用是创建SqlSession。

> SqlSession：Mybatis中又一个十分重要的对象，他是应用程序和持久层之间执行交互操作的一个单线程对象，其重要作用是执行持久化操作。

#### 工具类

```java
public class MyBatisUtils {
	//定义sqlSessionFactory对象
	private static SqlSessionFactory sqlSessionFactory = null;
	//初始化对象的值
	static {
		try {
			InputStream inputStream = Resources.getResourceAsStream("mybaits-config.xml");
			//2，根据配置文件构建SqlSession工厂类
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	//定义获取session对象的方法
	public static SqlSession getSession() {
		return sqlSessionFactory.openSession();
	}
}
```

#### SqlSession常用方法

```
// 查询方法
T  selectOne(String statement)
T  selectOne(String statement, Object param)
List<E> selectList(String statement)
List<E> selectList(String statement, Object param)
List<E> selectList(String statement, Object param, RowBounds rows)
// 插入，方法
int insert(String statement)
int insert(String statement, Object param)
// 更新方法
int update(String statement)
int update(String statement, Object param)
// 删除方法
int delete(String statement)
int delete(String statement, Object param)
void commit()		// 提交事务
void rollback()		// 回滚事务
void close()		// 关闭sqlSession对象
```

#### 配置文件(mybatis-config.xml)

##### 主要元素

![20181023164710401](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/20181023164710401.png)

> 注：
>
> 1. <configuration>是根元素其他元素都在根元素中配置
> 2. <configuration>的子元素必须按照图中由上到下的顺序进行配置

##### properties元素

> <properties>是一个配置属性的元素，该元素通常用于将内部的配置外在化，即通过外部的配置来动态地替换内部定义的属性

1. 在src目录下，添加一个名为db.properties的配置文件

   ```properties
   jdbc.driver=com.mysql.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/systop_crm
   jdbc.username=root
   jdbc.password=root
   ```

2. 在MyBatis配置文件mybatis-config.xml中配置<properties>元素

   ```xml
   <properties resource="db.properties"/>
   ```

3. 修改配置文件中数据库连接的信息

   ```xml
   <dataSource type="POOLED">
   	<!-- 数据库驱动 -->
       <property name="driver" value="${jdbc.driver}"/>
       <!-- 连接数据库的url -->
       <property name="url" value="${jdbc.url}"/>
       <!-- 连接数据库的用户名 -->
       <property name="username" value="${jdbc.username}"/>
       <!-- 连接数据库的密码 -->
       <property name="password" value="${jdbc.password}"/>
   </dataSource>
   ```

   > 完成上述配置之后，dataSource中的连接数据库的4个属性值将会由db.properties文件中对应的值来动态替换

##### settings元素

> <settings>元素主要用于改变MyBatis运行时的行为，例如开启二级缓存、开启延迟加载等。

| 设置参数                  | 描述                                                         | 有效值                 | 默认值   |
| ------------------------- | ------------------------------------------------------------ | ---------------------- | -------- |
| cacheEnabled              | 该配置影响所有映射器中配置缓存的全局开关                     | true\|false            | false    |
| lazyLoadingEnabled        | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。在特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态 | true\|false            | false    |
| aggressiveLazyLoading     | 当启用时，对任意延迟属性的调用会使带有延迟加载属性的对象完整加载；反之，每种属性将会按需加载 | true\|false            | true     |
| multipleResultSetsEnabled | 是否允许单一语句返回多结果集（需要兼容驱动）                 | true\|false            | true     |
| useColumnLabel            | 使用列标签代替列名。不同的驱动会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果 | true\|false            | true     |
| useGeneratedKeys          | 允许JDBC 支持自动生成主键，需要驱动兼容。如果设置为 true，则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作 | true\|false            | false    |
| autoMappingBehavior       | 指定 MyBatis 应如何自动映射列到字段或属性。NONE 表示取消自动映射；PARTIAL 表示只会自动映射，没有定义嵌套结果集和映射结果集；FULL 会自动映射任意复杂的结果集（无论是否嵌套） | NONE、PARTIAL、FULL    | PARTIAL  |
| defaultExecutorType       | 配置默认的执行器。SIMPLE 是普通的执行器；REUSE 会重用预处理语句（prepared statements）；BATCH 执行器将重用语句并执行批量更新 | SIMPLE、REUSE、BATCH   | SIMPLE   |
| defaultStatementTimeout   | 设置超时时间，它决定驱动等待数据库响应的秒数                 | 任何正整数             | 没有设置 |
| mapUnderscoreToCamelCase  | 是否开启自动驼峰命名规则映射                                 | true\|false            | false    |
| jdbcTypeForNull           | 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER | NULL、VARCHAR 、 OTHER | OTHER    |

- 使用方式：

  ```xml
  <settings>
      <setting name="cacheEnabled" value="true"/>
      <setting name="lazyLoadingEnabled" value="true"/>
      <setting name="multipleResultSetsEnabled" value="true"/>
      <setting name="useColumnLabel" value="true"/>
      <setting name="useGeneratedKeys" value="false"/>
      <setting name="autoMappingBehavior" value="PARTIAL"/>
      <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
      <setting name="defaultExecutorType" value="SIMPLE"/>
      <setting name="defaultStatementTimeout" value="25"/>
      <setting name="defaultFetchSize" value="100"/>
      <setting name="safeRowBoundsEnabled" value="false"/>
      <setting name="mapUnderscoreToCamelCase" value="false"/>
      <setting name="localCacheScope" value="SESSION"/>
      <setting name="jdbcTypeForNull" value="OTHER"/>
      <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
  </settings>
  ```

- 重要的有：

  ```xml
  <settings>
  	<!-- 开启缓存 -->
      <setting name="cacheEnabled" value="true"/>
      <!-- 开启懒加载 -->
      <setting name="lazyLoadingEnabled" value="true"/>
      <!-- 开启驼峰命名法 -->
      <setting name="mapUnderscoreToCamelCase" value="true"/>
  </settings>
  ```

##### typeAliases元素

> <typeAliases>元素用于为配置文件中的Java类设置以简单的名字，即设置别名

- 使用方式：

  ```xml
  <typeAliases>
  	<!-- 扫描单个类 -->
  	<typeAlias alias="customer" type="com.systop.pojo.Customer" />
  	<!-- 扫描单个类 -->
  	<typeAlias type="com.systop.pojo.Customer" />
  	<!-- 扫描包 -->
  	<typeAlias type="com.systop.pojo" />
  </typeAliases>
  ```

  > 注：
  >
  > 1. alisa属性的属性值customer，就是com.systop.pojo.Customer的自定义别名，它可以代替com.systop.pojo.Customer使用在任何位置
  > 2. 若是省略alias，则默认将类名首字母小写后的名称作为别ing
  > 3. 若是扫描包，则将包下所有pojo类以首字母小写后的非限定名作为它的别名

##### typeHandler元素

> MyBatis在预处理语句（PreparedStatement）中设置一个参数或从结果集（ResultSet）中取出一个值时，都会用其框架内部注册了的typeHandler（类处理器）进行相关处理。typeHandler的作用就是将预处理语句中传入的参数从JavaType（Java类型）转换为jdbcType（JDBC类型），或者从数据库取出结果时将jdbcType转换为JavaType。
>
> 注：MyBatis框架提供了一些默认的类处理器，我们其实不用做任何更改，可以注册新的类处理器

- 使用方式：

  ```xml
  <typeHandlers>
  	<!-- 注册一个类的类处理器 -->
  	<typeHandler handler="com.systop.type.CustomtypeHandler" />
  	<!-- 注册包中所有的类处理器 -->
  	<typeHandler handler="com.systop.type" />
  <typeHandlers/>
  ```

##### objectFactory元素

> MyBatis框架每次创建结果对象的新实例时，都会使用一个对象工厂（ObjectFactory）的实例来完成。MyBatis中默认的objectFactory的作用就是实例化目标类，它既可以通过默认构造方法实例化，也可以在参数映射存在的时候通过参数构造方法来实例化。
>
> 注：这个一般情况下也不用改

- 使用方法：

  1. 自定义一个工厂对象，需要实现ObjectFactory接口，或继承DefaultObjectFactory类

     ```java
     public class MyObjectFactory extends DefaultObjectFactory{
         private static final long serialVersionUID = -4114845625429965832L;
         // 重新构造其他方法
     }
     ```

  2. 在配置文件中配置自定义的ObjectFactory

     ```xml
     <objectFactory type="com.systop.factory.MyObjectFactory">
     	<propety name="name" value="MyObjectFactory">
     <objectFactory />
     ```

##### plugins元素

> MyBatis允许在已映射语句执行过程中的某一点进行拦截调用，这种拦截调用是通过插件来实现的。<plugins>元素的作用就是配置用户所开发的插件。
>
> 注：加载新的插件可能会破坏MyBatis原有的核心模块

##### environment元素

> 在配置文件中，<environment>元素用于对环境进行配置。MyBatis的环境配置实际上就是数据源的配置，我们可以通过<environment>元素配置多种数据源，即配置多种数据库。

- 使用方法

  ```xml
  <!-- 默认使用mysql数据源 -->
  <environments default="mysql">
      <!-- mysql数据源配置 -->
      <environment id="mysql">
          <!-- 使用JDBC事务管理 --> 
          <transactionManager type="JDBC" />
          <!-- 配置数据源 -->
          <dataSource type="POOLED">
              <property name="driver" value="${jdbc.driver}"/>
              <property name="url" value="${jdbc.url}"/>
              <property name="username" value="${jdbc.username}"/>
              <property name="password" value="${jdbc.password}"/>
          </dataSource>
      </environment>
      ...
  </environments>
  ```

  > MyBatis框架提供了UNPOOLED、POOLED和JNDI三种数据源类型，这里只介绍POOLED的操作，其他配置请自行查询学习

##### mappers元素

> 在配置文件中，<mappers>元素用于指定MyBatis映射文件的位置

- 使用方法：

  ```xml
  <mappers>
      <!-- 使用类路径引入 常用 -->
      <mapper resource="com/systop/mapper/CustomerMapper.xml" />
      <!-- 使用包名引入 常用 -->
      <package name="com.systop.mapper" />
      <!-- 使用接口类引入 不常用 -->
      <mapper class="com.systop.mapper.CustomerMapper" />
      <!-- 使用文件路径引入 不用 -->
      <mapper url="file:///D:com/systop/mapper/CustomerMapper.xml" />
  </mappers>
  ```

##### 简单配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 引入外部配置文件 -->
	<properties resource="db.properties" />
	
	<settings>
        <!-- 缓存：开 -->
		<setting name="cacheEnabled" value="true"/>
        <!-- 懒加载：开 -->
		<setting name="lazyLoadingEnabled" value="true"/>
        <!-- 驼峰命名法：开 -->
		<setting name="mapUnderscoreToCamelCase" value="true"/>
	</settings>

	<typeAliases>
        <!-- 自定义别名 -->
		<typeAlias type="com.systop.pojo.Customer" alias="cust"/>
	</typeAliases>
    
    <!--<typeHandlers>
		
		<typeHandlers />-->
    
    <!--<objectFactory>
		
		<objectFactory />-->

	<!--<plugins>
		
		</plugins>-->

	<!-- 默认使用mysql数据源 -->
	<environments default="mysql">
    	<!-- mysql数据源配置 -->
    	<environment id="mysql">
        	<!-- 使用JDBC事务管理 --> 
        	<transactionManager type="JDBC" />
        	<!-- 配置数据源 -->
        	<dataSource type="POOLED">
            	<property name="driver" value="${jdbc.driver}"/>
            	<property name="url" value="${jdbc.url}"/>
            	<property name="username" value="${jdbc.username}"/>
            	<property name="password" value="${jdbc.password}"/>
        	</dataSource>
    	</environment>
    	...
	</environments>
	
	<mappers>
		<mapper resource="com/systop/mapper/CustomerMapper.xml"/>
		<!-- <package name="com.systop.mapper"/> -->
	</mappers>
    
</configuration>
```

#### 映射文件(mapper.xml)

##### 主要元素

![1604290197511](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604290197511.png)

##### select元素

- 属性

  | 属性          | 说                                                           |
  | ------------- | ------------------------------------------------------------ |
  | id            | 表示命名空间内唯一标识符，常与命名空间组合起来使用           |
  | parameterType | 将传入该select语句的参数类型的完全限定名或别名               |
  | resultType    | 该条select语句将要返回的类型的完全限定名或别名               |
  | resultMap     | 外部resultMap的命名引用，不可和resultType同时使用            |
  | flushCache    | 设置为true时，则在任何时候只要语句被调用，都会导致本地缓存和二级缓存都被清空，默认为false。 |
  | useCache      | 设置为true时，将会导致本条语句的结果被二级缓存，默认为true   |
  | timeout       | 驱动程序等待数据库返回请求结果的秒数，默认值为unset(依赖驱动) |
  | fetchSize     | 尝试影响驱动程序每次批量返回的结果行数和这个设置值相等，默认为unset(依赖驱动) |
  | statementType | 值为STATEMENT、PREPARED、CALLABLE。默认值为PREPARED          |
  | resultSetType | 结果集的类型，值为FORWARD_ONLY、SCROLL_SENSITIVE、SCROLL_INSENSITIVE。默认为unset(依赖驱动) |

  > 注：经常使得有id、parameterType、resultType、resultMap

- 使用方式：

  ```xml
  <select id="findCustomerById" paramaterType="Interger" 
  	resultType="com.systop.pojo.Customer">
  	select * from customer where cust_id = #{id}
  </select>
  ```

##### insert、update和delete元素

- 属性：

  > 与select元素的属性大致一样，insert和update元素多了几种属性，重要的有keyProperty：（仅对insert和update有用）通常设置主键对应的属性。如果设置联合主键，可以在多个值之间用逗号隔开

- 使用方式：

  ```xml
  <insert id="insertCustomer" parameterType="com.systop.pojo.Customer" 
          keyProperty="cust_id" >
  	insert into customer(cust_id, cst_name, cust_phone, cust_address)
      value(#{cust_id}, #{cst_name}, #{cust_phone}, #{cust_address})
  </insert>
  <update id="updateCustomer" parameterType="com.systop.pojo.Customer" 
          flushCache="true" keyProperty="cust_id">
  	<!-- 更新语句 -->
  </update>
  <delete id="deleteCustomer" parameterType="Integer"
          flushCache="true">
  	delete from customer where cust_id = #{id}
  </delete>
  ```

##### sql元素

> sql元素标签的作用是储存多次使用的sql语句字段，以减少代码的臃肿

- 使用方法：

  ```xml
  <sql id="customerComlums">
  	cust_id,cust_name,cust_phone,cust_address
  </sql>
  
  <select id="findCustomerById" paramaterType="Interger" 
  	resultType="com.systop.pojo.Customer">
  	select <include refid="customerComlums" />
      from customer 
      where cust_id = #{id}
  </select>
  ```

  > 上述代码是sql元素标签的简单使用方式

  ```xml
  <sql id="tablename">
  	${prefix}omter
  </sql>
  <!-- 定义要查询的表 -->
  <sql id="someinclude">
  	from
      <include tefid="${include_target}" />
  </sql>
  <!-- 定义查询列 -->
  <sql id="customerComlums">
  	cust_id,cust_name,cust_phone,cust_address
  </sql>
  
  <select id="findCustomerById" paramaterType="Interger" 
          resultType="com.systop.pojo.Customer">
      select
      <include refid="customerComlums" />
      <include refid="someinclude">
          <property name="include_target" value="tablename" />
      	<property name="prefix" value="cust" />
      </include>
      where cust_id = #{cust_id}
  </select>
  ```

##### resultMap元素

> <resultMap>元素表示结果映射集，是MyBatis中最重要也是最强大的元素。它的主要功能是定义映射规则、级联的更新以及定义类转换器等

<resultMap>元素中包含了一些子元素，它的元素结构大致如下：

```xml
<!-- resultMap元素结构 -->
<resultMap type="" id="">
	<constructor>		<!-- 类实例化时，用来注入结果到构造方法中 不常用 -->
    	<idArg/>		<!-- ID参数；标记结果作为ID 不常用 -->
        <arg/>			<!-- 注入到构造方法的一个普通结果 不常用 -->
    </constructor>
    <id property="" column=""/>				<!-- 用于表示哪一列是主键 常用 -->
    <result property="" column=""/>			<!-- 注入到字段或JavaBean属性的普通结果 常用 -->
    <association property="" column="" javaType="" select=""/>		<!-- 用于表的一对一关联 常用 -->
    <collection property="" column="" ofType="" select=""/>		<!-- 用于表的一对多关联 常用 -->
    <discriminator javaType="">		<!-- 使用结果值来决定使用哪个结果映射 不常用 -->
    	<case value="" />			<!-- 基于某些值的结果映射 不常用 -->
    </discriminator>
</resultMap>
```

> 注：在后面MyBatis的关系映射中会专门讲它的使用
>
> 1. property：传入pojo对象的属性名
> 2. colum：传入数据库对应的字段名
> 3. javaType：传入表外键对应的pojo类（全路径限定名或别名）
> 4. ofType：传入表外键对应的pojo类（全路径限定名或别名）
> 5. select：mybatis映射文件对应的查询语句
> 6. association：用于表的一对一关联
> 7. collection：用于表的一对多关联

##### 动态sql

- 元素

  | 元素                      | 说明                                                         |
  | ------------------------- | ------------------------------------------------------------ |
  | <if>                      | 判断语句，用于单条件分支判断                                 |
  | <choose><when><otherwise> | 相当于Java中的switch...case...defullt语句，用于多条件分支判断 |
  | <where><trim><set>        | 辅助元素，用于处理一些SQL拼装、特殊字符问题                  |
  | <foreach>                 | 循环语句，常用于in语句等列举条件中                           |
  | <bind>                    | 从OGNL表达式中创建一个变量，并将其绑定到上下文，常用于模糊查询的sql中 |

- <if>元素

  场景：根据客户姓名和地址组合条件查询客户信息列表

  ```xml
  <!-- 根据客户姓名和地址查询语句 -->
  <select id="findCustomerByNameAndAddress" parameterType="cust" resultType="cust">
      select * from customer where 1=1 
      <if test="cust_name != null and cust_name != ''">
      	and cust_name like concat('%',#{cust_name},'%')
      </if>
      <if test="cust_address != null and cust_address != ''">
      	and cust_address = #{cust_address}
      </if>
  </select>
  ```

- <choose>、<when>、<otherwise>元素

  场景：当客户名字不为空，则使用名称查询，名字为空则使用地址查询，如果都为空则使用手机号查询

  ```xml
  <!-- 根据客户姓名、地址、手机号查询语句 -->
  <select id="findCustomerByNameOrAddress" parameterType="cust" resultType="cust">
      select * from customer where 1=1 
      <choose>
          <when test="cust_name != null and cust_name != ''">
              and cust_name like concat('%',#{cust_name},'%')
          </when>
          <when test="cust_address != null and cust_address != ''">
              and cust_address = #{cust_address}
          </when>
          <otherwise>
              and cust_phone is not null
          </otherwise>
      </choose>
  </select>
  ```

- <where>、<trim>元素

  场景： 根据客户姓名和地址查询语句

  ```xml
  <!-- 根据客户姓名和地址查询语句  where  -->
  <select id="findCustomerByNameAndAddress" parameterType="cust" resultType="cust">
      select * from customer
      <where>
          <if test="cust_name != null and cust_name != ''">
              and cust_name like concat('%',#{cust_name},'%')
          </if>
          <if test="cust_address != null and cust_address != ''">
              and cust_address = #{cust_address}
          </if>
      </where>
  </select>
  ```

  ```xml
  <!-- 根据客户姓名和地址查询语句  trim  -->
  <select id="findCustomerByNameAndAddress" parameterType="cust" resultType="cust">
      select * from customer
      <trim prefix="where" prefixOverrides="and">
          <if test="cust_name != null and cust_name != ''">
              and cust_name like concat('%',#{cust_name},'%')
          </if>
          <if test="cust_address != null and cust_address != ''">
              and cust_address = #{cust_address}
          </if>
      </trim>
  </select>
  ```

- <set>元素

  场景：根据客户传参信息更新姓名，地址，电话三个信息

  ```xml
  <!-- 根据客户传参信息更新姓名，地址，电话三个信息 -->
  <update id="updateCustomer" parameterType="cust">
      update customer
      <set>
          <if test="cust_name != null and cust_name != ''">
              cust_name = #{cust_name},
          </if>
          <if test="cust_address != null and cust_address != ''">
              cust_address = #{cust_address},
          </if>
          <if test="cust_phone != null and cust_phone != ''">
              cust_phone = #{cust_phone},
          </if>
      </set>
      where cust_id = #{cust_id}
  </update>
  ```

  > 注：
  >
  > 1. where元素在其内部if子语句判断为true时自动在前面拼接一个“where”，并将第一个判断为true的if子语句中的首字母and去除，若无and则不去除
  > 2. trim元素与where元素类似，当子语句判断为true时自动在前面拼接prefix的值，并去除prefixOverrides的值，若无则不去除
  > 3. set元素也是如此，当子语句判断为true时在前面拼接一个“set”，他不去除任何字符

- <forEach>元素

  情景：根据客户查询的多个id查询用户

  ```xml
  <!-- 根据多个客户id查询用户 -->
  <select id="findCustomerByIds" parameterType="List" resultType="cust">
      select * from customer where cust_id in 
      <foreach item="cust_id" index="index" collection="list" 
               open="(" separator="," close=")">
          #{cust_id}
      </foreach>
  </select>
  ```

  > 注：
  >
  > 1. item：循环中当前元素的名称
  > 2. index：当前元素在集合的位置下标
  > 3. collection：参数类型，它可以是list、array、Map集合的键、pojo包装类中数组或集合类型的属性名等
  > 4. Open: 开始前添加的符号
  > 5. Close: 结束后添加的符号
  > 6. separator：各个元素的间隔符

- <bind>元素

  情景：根据客户模糊查询

  ```xml
  <!-- 根据客户模糊查询  -->
  <select id="findCustomerByName" parameterType="cust" resultType="cust">
      <bind name="pattern_cust_name" value="'%'+cust_name+'%'" />
      select * from customer where cust_name like #{pattern_cust_name}
  </select>
  ```

### MyBatis的关系映射

> 在关系型数据库中，多表之间存在着三种关联关系，一对一、一对多和多对多

![1604387658739](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604387658739.png)

三种关联关系的具体说明如下：

- 一对一：在任意一方引入对方主键作为外键。
- 一对多：在“多”的一方，添加“一”的一方的主键作为外键。
- 多对多：产生中间关系表，引入两张表的主键作为外键，两个主键成为联合主键或使用新的字段作为主键。

通过数据库的表可以描述数据之间的关系，同样，在Java中，通过对象也可以进行关联关系描述：

![1604388013915](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604388013915.png)

- 一对一的关系：就是在本类中定义对方类型的对象，如A类中定义B类类型的属性b，B类中定义A类类型的属性a。
- 一对多的关系：就是一个A类类型对应多个B类类型的情况，需要在A类中以集合的方式引入B类类型的对象，在B类中定义A类类型的属性a。
- 多对多的关系：在A类中定义B类类型的集合，在B类中定义A类类型的集合。

#### 一对一

> 案例：在现实生活中，一对一关联关系是十分常见的。例如，一个人只能有一个身份证，同时一个身份证也只会对应一个人

- <resultMap>元素中，包含了一个<association>子元素，MyBatis就是通过该元素来处理一对一关联关	系的。

- 在<association>元素中，通常可以配置以下属性:

  - **property：**指定映射到的实体类对象属性，与表字段一一对应；
  - **column：**指定表中对应的字段；
  - **javaType：**指定映射到实体对象属性的类型；
  - **select：**指定引入嵌套查询的子SQL语句，该属性用于关联映射中的嵌套查询；
  - **fetchType：**指定在关联查询时是否启用延迟加载。该属性有lazy和eager两个属性值，默认值为lazy（即默认关联映射延迟加载）

  ```xml
  <!-- 嵌套查询 -->
  <association property="card" column="card_id" javaType="IdCard"
  		 select="com.systop.mapper.IdCardMapper.findCodeById" />
  <!-- 嵌套结果 -->
  <association property="card" javaType="IdCard">
      <id property="id" column="card_id"/>
      <result property="code" column="code"/>
  </association>
  ```

- 代码：

  **数据库：**

  [test.sql](https://pan.baidu.com/s/1eRssoBia1fVuB8zt7UTQJQ "提取码：carp")

  **pojo类**

  ```java
  package com.systop.po;
  
  public class IdCard {
  	private Integer id;
  	private String code;
  
  	public String toString() {
  		return "IdCard [id=" + id + ", code=" + code + "]";
  	}
  
      // ...    
  }
  ```

  ```java
  package com.systop.po;
  
  public class Person {
  	private Integer id;
  	private String name;
  	private Integer age;
  	private String sex;
  	// 一对一的对象
  	private IdCard card;
  
  	public String toString() {
  		return "Person [id=" + id + ", name=" + name + ", age=" + age + ", "
  				+ "sex=" + sex + ", card=" + card + "]";
  	}
  
      // ...
  ```

  **Mapper.xml**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
  <!-- 命名空间：完全路径 -->
  <mapper namespace="com.systop.mapper.IdCardMapper">
  	<!-- 根据Id查询身份证 -->
  	<select id="findCodeById" parameterType="Integer" resultType="idCard">
  		select * from tb_idcard where id = #{id}
  	</select>
  </mapper>
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
  <!-- 命名空间：完全路径 -->
  <mapper namespace="com.systop.mapper.PersonMapper">
      <!-- 嵌套查询 -->
  	<!-- 结果集 -->
  	<resultMap id="PersonAndCardResult1" type="person">
  		<id property="id" column="id" />
  		<result property="name" column="name" />
  		<result property="age" column="age" />
  		<result property="sex" column="sex" />
  		<!-- 一对一 ：association使用select属性引入另外一个sql语句 -->
  		<association property="card" column="card_id" javaType="IdCard"
  		 select="com.systop.mapper.IdCardMapper.findCodeById" />
  	</resultMap>
  	<!-- 根据Id查询人员信息  -->
  	<select id="findPersonById" parameterType="Integer" 
              resultMap="PersonAndCardResult1">
  		select * from tb_person where id=#{id}
  	</select>
      
      <!-- 嵌套结果 -->
      <!-- 结果集 -->
  	<resultMap id="PersonAndCardResult2" type="person">
  		<id property="id" column="id" />
  		<result property="name" column="name" />
  		<result property="age" column="age" />
  		<result property="sex" column="sex" />
  		<!-- 一对一 ：association使用select属性引入另外一个sql语句 -->
  		<association property="card" javaType="IdCard">
              <id property="id" column="card_id"/>
              <result property="code" column="code"/>
          </association>
  	</resultMap>
      <!-- 根据Id查询人员信息  -->
  	<select id="findPersonById" parameterType="Integer" 
              resultMap="PersonAndCardResult2">
  		select p.*,i.code 
          from tb_person p,tb_idcard i 
          where p.card_id=i.id and p.id=#{id}
  	</select>
  </mapper>
  ```

  **main类**

  ```java
  public static void main(String[] args) {
      SqlSession sqlSession = MyBatisUtils.getSession();
      Person person = sqlSession.selectOne("com.systop.mapper.PersonMapper.findPersonById",2);
      System.out.println(person);
      sqlSession.close();
  }
  ```

  > 注：
  >
  > 1. 在mybatis-config.xml文件中开启延迟加载，而MyBatis中的<association>元素和<collection>元素中都已默认配置了延迟加载属性，无需再做配置
  > 2. 如果使用嵌套结果查询，则无法使用延迟加载，故推荐使用嵌套查询

#### 一对多

> <resultMap>元素中，包含了一个<collection>子元素，MyBatis就是通过该元素来处理一对多关联关系的。<collection>子元素的属性大部分与<association>元素相同，但其还包含一个特殊属性--ofType 。
>
> ofType：ofType属性与javaType属性对应，它用于指定实体对象中集合类属性所包含的元素类型。

```xml
<!-- 方法1：嵌套查询 -->
<collection property="ordersList" column="id" ofType="Orders"
		select="com.systop.mapper.OrderMapper.selectOrders"/>
<!-- 方法2：嵌套结果 -->
<collection property="ordersList" ofType="Orders">
	<id property="id" column="orders_id" />
	<result property="number" column="number" />
</collection>
```

#### 多对多

> 多对多相当于双份的一对多也是使用<resultMap>元素

在实际项目开发中，多对多的关联关系也是非常常见的。以订单和商品为例，一个订单可以包含多种商品，而一种商品又可以属于多个订单。

![1604391472093](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604391472093.png)

在数据库中，多对多的关联关系通常使用一个中间表来维护，中间表中的订单id作为外键参照订单表的id，商品id作为外键参照商品表的id。

![1604391420122](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604391420122.png)

**至此，MyBatis的学习就暂告一段落。**

## Spring

> Spring是分层的JavaSE/EE full-stack（一站式）轻量级开源框架，以IoC（Inverse of Control 控制反转）和AOP（Aspect Oriented Programming 面向切面编程）为内核，使用基本的JavaBean来完成以前只可能由EJB完成的工作，取代了EJB的臃肿、低效的开发模式。

Spring具有简单、可测试和松耦合等特点。Spring不仅可以用于服务器端开发，也可以应用于任何Java应用的开发中。

- **非侵入式设计**

  Spring是一种非入侵式（non-invasive）框架，它可以使应用程序代码对框架的依赖最小

- **方便解耦、简化开发**

  Spring就是一个大工厂，可以将所有对象创建和依赖的关系维护，交给Spring管理

- **支持AOP**

  Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能

- **支持声明式事务处理**

  只需要通过配置就可以完成对事务的管理，而无需手动编程

- **方便程序测试**

  Spring对Junit4支持，可以通过注解方便的测试Spring程序

- **方便集成各种优秀框架**

  Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架的直接支持（如：Struts、Hibernate、MyBatis等）

- **降低Java EE API的使用难度**

  Spring对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低

### Spring的体系结构

> Spring框架采用的是分层架构，它一系列的功能要素被分成20个模块

![1604393191905](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604393191905.png)

- **核心容器（Core Container）**

  - **Beans模块：**提供了BeanFactory，是工厂模式的经典体现，Spring将管理对象称为Bean
  - **Core核心模块：**提供了Spring框架的基本组成部分，包括Ioc和DI功能
  - **Context上下文模块：**建立在Core和Beans模块的基础之上，它是访问定义和配置的任何对象的媒介。其中ApplicationContext接口是上下文模块的焦点
  - **Context-support模块：**提供了对第三方库关于Spring应用的集成支持，比如缓存、邮件服务、任务调度和模版引擎
  - **SpEL模块：**是Spring3.0后新增的模块，它提供了Spring Expression Language支持，是运行时查询和操作对象图的强大的表达式语言（相当于spring版的EL表达式，不重点学习）

- **数据访问/集成（Data Access/Integration）层**

  包括JDBC、DRM、JMS和Transaction模块，除Transaction模块之外都不重点涉及

  - Transaction模块：支持对实现特殊接口以及所有的pojo类编程和声明式的事务管理

- **Web层**

- **AOP（Aspect Oriented Programming）模块**

  提供了面向切面编程实现，允许定义方法拦截和切入点，将代码按照功能进行分离，以降低耦合性

- **Aspects模块**

  提供了与AspectJ的集成功能，AspectJ是一个功能强大的面向切面框架

- **植入（Instrumentation）模块**

- **消息传输（Messaging）**

- **测试（Test）模块**

### Spring的入门程序

- jar包

  spring-beans-4.3.6.RELEASE.jar

  spring-context-4.3.6.RELEASE.jar

  spring-core-4.3.6.RELEASE.jar

  spring-expression-4.3.6.RELEASE.jar

  commons-logging-1.2.jar

- 在src目录下创建一个applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans 
  	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
  	<!-- 该文件是Spring管理对象的文件 -->
  	
  	<!-- 把UserDao类型对象交给Spring管理 -->
  	<bean id="userDao" class="com.systop.ioc.UserDao"/>
  	<!-- 把UserServiceImpl类型对象交给Spring管理 -->
  	<bean id="userService" class="com.systop.ioc.UserService">
  		<property name="userDao" ref="userDao"  />
  		<property name="age" value="20" />
  	</bean>
  </beans>
  ```

- Java类

  ```java
  package com.systop.ioc;
  
  public class UserDao{
  	public void sayHi() {
  		System.out.println("你好，Hello World");
  	}
  }
  ```

  ```java
  package com.systop.ioc;
  
  public class UserService {
  	//属性
  	private UserDao userDao;
  	private int age;
  	
  	@Override
  	public void say() {
  		System.out.println("这是Service层逻辑");
  		System.out.println("调用say方法:age="+age);
  		System.out.println("====================");
  		userDao.sayHi();
  	}
  
  	public UserDao getUserDao() {
  		return userDao;
  	}
  	public void setUserDao(UserDao userDao) {
  		this.userDao = userDao;
  	}
  	public int getAge() {
  		return age;
  	}
  	public void setAge(int age) {
  		this.age = age;
  	}
  }
  ```

  ```java
  package com.systop.ioc;
  
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Test {
  	public static void main(String[] args) {
  		//传统实例化对象的方式
  		UserDao userDao = new UserDao();
  		userDao.sayHi();
  		//Spring方式来实例化对象
  		ApplicationContext app = 
              new ClassPathXmlApplicationContext("applicationContext.xml");
  		UserDao userDao = (UserDao)app.getBean("userDao");
  		userDao.sayHi();
  		
  		//多层关系  传统调用
  		UserService userService = new UserService();
          userService.setUserDao(userDao);
          userService.setAge(12);
  		userService.say();
  		//多层关系  Spring方式来实例化对象
  		ApplicationContext app = 
              new ClassPathXmlApplicationContext("applicationContext.xml");
  		UserService userService = (UserService)app.getBean("userService");
  		userService.say();
  	}
  }
  ```

  > 注：上述程序中出现了工厂模式创建对象，控制反转，依赖注入

### Spring的核心容器

> Spring框架的主要功能是通过其核心容器来实现的，我们有必要对其核心容器有一定的了解。Spring框架提供了两种核心武器，分别为BeanFactory（对象工厂）和ApplicationContext（全局上下文）

- **BeanFactory**

  **BeanFactory由org.springframework.beans.factory.BeanFactory接口定义，是基础类型的Ioc容器（控制反转）**，它提供了完整的IoC服务支持。简单来说，Beanfactory就是一个管理Bean的工厂，他主要负责初始化各种Bean，并调用它们的生命周期。

  ```java
  BeanFactory beanFactory = 
      new XmlBeanFactory(new FileSystemResource("F:/applicationContext.xml"));
  ```

  > 注：参数是文件系统路径（非项目系统路径）

  这种加载方式在开发中并不多用（老得不能再老的系统），读者了解即可

- **ApplicationContext**

  ApplicationContext也是BeanFactory的子接口，称为应用上下文，是另外一种常用的Spring核心容器。他由org.springframewokr.context.AppliactionContext接口定义，**他不仅仅包含了BeanFactory的所有功能，还添加了对国际化、资源访问、事件传播等方面的技术。**

  ```java
  // 1、通过ClassPathXmlApplicationContext创建
  ApplicationContext app = 
      new ClassPathXmlApplicationContext("applicationContext.xml")
  ```

  > 注：文件在项目中的绝对路径

  ```java
  // 2、通过FileSystemXmlApplicationContext创建
  ApplicationContext app = 
      new FileSystemXmlApplicationContext("F:/applicationContext.xml");
  ```

  > 注：参数是文件系统路径

  Spring获取Bean的实例通常采用以下两种方法：

  - `Object  getBean(String name)` ：根据容器中的Bean的id或name来获取指定的Bean，获取值后需要进行数据类型转换。
  - `<T>  T  getBean(Class<T> requiredType)` ：根据累的类型来获取Bean的实例。此方法为泛型方法，因此在获取Bean之后不需要进行强制类型转换。

### 控制反转与依赖注入

> IOC-Inversion of Control，译为控制反转，是一种遵循依赖倒置原则的代码设计思想。

　　所谓依赖倒置，就是把原本的高层建筑依赖底层建筑“倒置”过来，变成底层建筑依赖高层建筑。高层建筑决定需要什么，底层去实现这样的需求，但是高层并不用管底层是怎么实现的。这样就不会出现前面的“牵一发动全身”的情况。
　　而控制反转就是把传统程序中需要实现对象的创建、代码的依赖，反转给一个专门的"第三方"即容器来实现，即将创建和查找依赖对象的控制权交给容器，由容器将对象进行组合注入，实现对象与对象的松耦合，便于功能的复用，使程序的体系结构更灵活。

![1604404992851](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604404992851.png)

​	DI-Dependency Injection,译为依赖注入，实际上是对Ioc的另一种称呼。2004年，大师级人物Martin Fowler为“控制反转”取了一个更合适的名字“依赖注入”。给出了实现IOC的方法：注入。所谓依赖注入，就是由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中。

​	当某个Java对象（调用者）需要调用另一个Java对象（被调用者，即被依赖对象）时，在传统模式下，调用者通常会采用“new 被调用者”的代码方式来创建对象，这种方式会导致调用者与被调用者之间的耦合度增加，不利于后期项目的升级和维护。

​	在使用Spring框架之后，对象的实例不再由调用者来创建，而是由Spring容器来创建，Spring容器会负责控制程序之间的关系，而不是由调用者的程序代码直接控制。这样控制权由应用代码转移到了Spring容器，控制权发生了反转，这就是Spring的控制反转。

​	从Spring容器的角度来看，Spring容器负责将被依赖对象赋值给调用者的成员变量，这相当于为调用者注入了它依赖的实例，这就是Spring的依赖注入。

理解IOC和DI的关键：

　　--控制反转，谁控制了谁？

​	--IOC容器控制了对象

​	--控制了什么？

​	--控制了外部资源的获取（对象，文件等）

​	--哪些方面的控制被反转了？

​	--获得被依赖对象（被调用者）的过程被反转了。

　　--依赖注入，谁依赖谁？

​	--程序依赖IOC容器

​	--为什么需要依赖？

​	--程序需要IOC容器来提供外部资源

​	--谁注入谁？

​	--IOC容器向程序注入

​	--注入了什么？

​	--注入某个对象所需要的外部资源。

　　依赖注入(DI)和控制反转(IOC)是从不同的角度的描述的同一件事情，就是指通过引入IOC容器，利用依赖关系注入的方式，实现对象之间的解耦。

- **依赖注入的实现方式**

  > 依赖注入的实现作用就是在使用Spring框架的时候，动态地将其所依赖的对象注入Bean组件中

  - setter方法注入

    通过调用无参构造器或无参静态工厂实例化Bean后，调用该Bean的setter方法来实现

  - 构造方法注入

    通过调用有参构造方法来实现

  在上面入门程序中userService就是通过setter方法注入的

> 注：在需要依赖注入的Bean内必须要创建setter方法或有参构造

### Spring中的Bean

> Spring可以被看作一个大型工厂，这个工厂的作用就是生产和管理Spring容器中的Bean对象

|                 |                                                              |
| --------------- | ------------------------------------------------------------ |
| **id**          | 是一个Bean的唯一标识符，Spring容器对Bean的配置、管理都通过该属性来完成 |
| name            | Spring容器也可以通过此属性来管理，name属性可以为Bean指定多个名称，名称之间用逗号隔开 |
| **class**       | 该属性指定了Bean的具体实现类，它必须全限定名：包+类名        |
| **scope**       | 用来定义Bean的实例作用域，singleton（单例）,prototype（原型），request，session，global Session、application和websocket，默认值为：singleton |
| constructor-arg | <Bean>元素的子元素，用于实现Bean的构造参数传递注入值         |
| **property**    | <Bean>元素的子元素，用于实现Bean的Setter注入时设置的属性     |
| **ref**         | <constructor-arg>、<property>属性，用于给某个属性赋值（引用类的值） |
| **value**       | <constructor-arg>、<property>属性，用于给某个属性赋值（简单类型的值） |
| list            | 用于封装List或数组类型的依赖注入                             |
| set             | 用于封装Set类型的依赖注入                                    |
| map             | 用于封装Map类型的依赖注入                                    |
| entry           | <map>元素的子元素，用于设置一个键值对。其key，ref或value     |

> 注：重要的属性有id、class、scope、property、ref、value

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
	<bean id="bean1" class="com.systop.test.Bean1" scope="prototype">
		<property name="" value=""/>
	</bean>
	<bean id="bean2" class="com.systop.test.Bean2">
		<property name="" ref="bean1"/>
	</bean>
</beans>
```

### Bean的生命周期

![1604450358918](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1604450358918.png)

1. Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化
2. Bean实例化后对将Bean的引入和值注入到Bean的属性中
3. 如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法
4. 如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
5. 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。
6. 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。
7. 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用
8. 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。
9. 此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。
10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。

> 注：可以先了解，不用太过深究

我们在了解了Xml配置文件装配Bean方式，下面我来介绍一种注解装配方式

### Bean基于Annotation的装配

- **@Component：**使用该注解描述Spring的Bean
- **@Repository：**数据访问层（DAO层）的Bean，其功能和@Component是一样的
- **@Service：**业务逻辑层（serivce层）的Bean，其功能和@Component是一样的
- **@Controller：**表现层（SpringMVC的controller层）的Bean，其功能和@Component是一样的
- **@Autowired：**用于对Bean的属性变量、属性的setter方法及构造方法进行标注，配合Bean完成自动配置工作。默认为按照Bean类型进行装配。
- **@Resource：**其作用和@Autowired一样，区别在于@Autowired是按照Bean的类型装配，而@Resuource是按照Bean实例名进行装配。有两个属性：name和type
- @Qualifier：与@Autowired注解配合使用，会将默认的按Bean类型装配改为Bean的实例化名称装配。

> 注：
>
> 1. @Component：可以应用在pojo类、dao层、service层、controller层
> 2. @Repository：应用在Dao层；@Service：应用在Service层；@Controller：应用在Controller层
> 3. @Autowired：应用在类的属性值上，它会自动匹配类型完成Bean的装配
> 4. @Resource：@Autowired是自动，@Resource是手动配置

- 配置文件：applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xmlns:context="http://www.springframework.org/schema/context"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans 
  	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
  	http://www.springframework.org/schema/context 
  	http://www.springframework.org/schema/context/spring-context-4.3.xsd">
  	<!-- 使用context命名空间，在配置文件中开启相应的注解 -->
  	<context:annotation-config />
      <!-- 定义扫包的范围 -->
  	<context:component-scan base-package="com.systop.annotation"/>
  </beans>
  ```

- 导入jar：spring-aop-4.3.6.RELEASE.jar

- java代码

  ```java
  package com.systop.annotation;
  
  public interface UserDao {
  	public void save();
  }
  ```

  ```java
  package com.systop.annotation;
  
  import org.springframework.stereotype.Repository;
  
  @Repository("userDao")
  public class UserDaoImpl implements UserDao {
  
  	@Override
  	public void save() {
  		System.out.println("userdao......save方法");
  	}
  }
  ```

  ```java
  package com.systop.annotation;
  
  public interface UserService {
  	public void save();
  }
  ```

  ```java
  package com.systop.annotation;
  
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;
  
  @Service("userService")
  public class UserServiceImpl implements UserService{
  
  	@Autowired
  	private UserDao userDao;
  	
  	@Override
  	public void save() {
  		System.out.println("userService.......save方法");
  		userDao.save();
  	}
  }
  ```

  ```java
  package com.systop.annotation;
  
  import javax.annotation.Resource;
  
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Component;
  
  @Component("userController")
  public class UserController {
  	
  	@Resource(name="")
  	private UserService userService;
  	
  	public void save() {
  		System.out.println("controller。。。。。。。。。save方法");
  		userService.save();
  	}
  }
  ```

  ```java
  package com.systop.annotation;
  
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Test {
  	public static void main(String[] args) {
  		String path = "com/systop/annotation/applicationContext.xml";
  		ApplicationContext app = new ClassPathXmlApplicationContext(path);
  		UserController userController = 
              (UserController)app.getBean("userController");
  		userController.save();
  	}
  }
  ```

  > 注：这是注解类的大致应用方式

### Spring Aop

> AOP（Aspect Oriented Programming）称为面向切面编程，AOP采取横向抽取机制，将分散各个方法中的重复代码提取出来，然后在程序编译或运行时，再将这些提取出来的代码应用到需要执行的地方。

- 现在Java流行的AOP：Spring AOP 和 AspectJ
- 术语：
  - Aspect（切面）：用于横向插入的代码类
  - Joinpoint（连接点）：切面代码类方法的调用
  - PointCut（切入点）：切面与程序交叉点，需要处理的那个连接点
  - Advice（通知/增强处理）：在切入点处所执行的切面代码类方法
  - Target Object（目标对象）：被切入的对象
  - Proxy （代理）：被动态创建的对象
  - Weaving（织入）：将切面代码插入到目标对象上，从而生成代理对象的过程

![1605096997579](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605096997579.png)

![1605097226749](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605097226749.png)

> Aop虽然十分重要，但我们不在此详细讲解代买的实现，只说明它的操作理念

### Spring的事务管理

> 事务是指是程序中一系列严密的逻辑操作，而且所有操作必须全部成功完成，否则在每个操作中所作的所有更改都会被撤消。可以通俗理解为：就是把多件事情当做一件事情来处理，好比大家同在一条船上，要活一起活，要完一起完 。

**事物的四个特性：**

- **原子性**（Atomicity）：操作这些指令时，要么全部执行成功，要么全部不执行。只要其中一个指令执行失败，所有的指令都执行失败，数据进行回滚，回到执行指令前的数据状态。
- **一致性**（Consistency）：事务的执行使数据从一个状态转换为另一个状态，但是对于整个数据的完整性保持稳定。
- **隔离性**（Isolation）：隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。
- **持久性**（Durability）：当事务正确完成后，它对于数据的改变是永久性的。

**PlatformTransactionManager**：Spring提供的事务管理器，主要作用管理事务、该接口提供3个方法：

- getTransaction(TransactionDefinition paramTransactionDefinition)
- commit(TransactionStatus paramTransactionStatus)
- rollback(TransactionStatus paramTransactionStatus)

**TransactionDefinition**：事务定义描述的对象，该接口定义了事务的规则，并提供了获取事务相关信息的方法

- **事务的传播形式**：

  - **PROPAGATION_REQUIRED** （required） ：如果当前没有事务，则开启一个新事务，如果当前有事务，则把该方法加入到已有事务中。（注意：增删改会用该策略）

  - **PROPAGATION_SUPPORTS**  (supports) ：如果当前方法有事务，则加入，如果没有则不开启新事务（注意：查询会用该策略）

    > 参数出了上述两个，还有5种自行查找

**TransactionStatus**：事务的状态，它描述了某一个时间点上事务的状态信息。

**事务管理的方式**

- 编程式事务管理：通过代码去实现事务管理，也就是程序要需要自己编写事务的开启，提交，回滚。
- **声明式事务管理**：是通过AOP技术实现的事务管理，把事务当切面横插入代码中

> 注：编程式在SpringMVC中不作为重点

例：编写一个简略的银行转账系统

所用的jar包：



![1605150129325](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605150129325.png)

1. **基于XML配置的声明式事务**

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
     	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
     	xmlns:aop="http://www.springframework.org/schema/aop"
     	xmlns:tx="http://www.springframework.org/schema/tx" 
     	xmlns:context="http://www.springframework.org/schema/context"
     	xsi:schemaLocation="http://www.springframework.org/schema/beans 
     	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
     	http://www.springframework.org/schema/aop
     	http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
     	http://www.springframework.org/schema/context
     	http://www.springframework.org/schema/context/spring-context-4.3.xsd
     	http://www.springframework.org/schema/tx
     	http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
     	
     	<!-- 1、数据源id固定名字不能写错dataSource -->
     	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
     		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
             <!-- 改数据库，用户名和密码 -->
     		<property name="url" value="jdbc:mysql://localhost:3306/test" />
     		<property name="username" value="root" />
     		<property name="password" value="root" />
     	</bean>
     	
     	<!-- 2、Spring的JDBC模板 -->
     	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
     		<!-- 注入属性：数据源 -->
     		<property name="dataSource" ref="dataSource" />
     	</bean>
     	
     	<!-- 3 、 定义Bean对象CardDao -->
     	<bean id="cardDao" class="com.systop.dao.impl.CardDaoImpl">
     		<property name="jdbcTemplate" ref="jdbcTemplate"/>
     	</bean>
     	
     	<bean id="cardService" class="com.systop.service.impl.CardServiceImpl">
     		<property name="cardDao" ref="cardDao"/>
     	</bean>
     	
     	<!-- 事务管理器 -->
     	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     		<property name="dataSource" ref="dataSource" />
     	</bean>
     	
     	<!-- 通知：对事务进行增强（通知），需要编写对切入点和具体执行事务细节 -->
         <!-- id:唯一标识,transaction-manager:指定事务管理器 -->
     	<tx:advice id="txAdvice" transaction-manager="transactionManager">
     		<tx:attributes>
                 <!-- name:"find*"是指以find开头的所有方法都加这个事务 -->
                 <!-- propagation:"SUPPORTS"/"REQUIRED"指定事务传播行为 -->
                 <!-- isolation:指定事务隔离级别 -->
                 <!-- read-only:"true"/"false"指定事务是否为只读 -->
     			<tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
     			<tx:method name="select*" propagation="SUPPORTS" read-only="true"/>
     			<tx:method name="add*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="save*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="insert*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="update*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="delete*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="remove*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     			<tx:method name="transfer" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
     		</tx:attributes>
     	</tx:advice>
     
     	<!-- 编写aop，让spring自动对目标生成代理，需要使用AspectJ的表达式 -->
     	<aop:config>
             <!-- 切入点 -->
             <!-- expression:execution(* com.systop.service.impl.*.*(..))
     			第一个星代表所有返回类型，第二个参数为com.systop.service.impl路径下的所有类的所有方法，无所谓参数类型 -->
     		<aop:pointcut expression="execution(* com.systop.service.impl.*.*(..))" id="txPointCut"/>
     		<!-- 切面：将切入点和通知整合 -->
             <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
     	</aop:config>
     </beans>
     ```

     > 上述配置文件我们操作了
     >
     > 1. 配置数据源
     > 2. 配置JDBC模版
     > 3. 配置事务管理器Bean
     > 4. 配置支持事务的方法和事务传播行为、隔离级别和是否为只读
     >    1. 编写aop，生成代理
     >
     > 注：代理依赖通知，通知依赖事务管理器，事务管理器依赖数据源

   - com.systop.dao.CardDao.java

     ```java
     package com.systop.dao;
     
     public interface CardDao {
     	
     	public int changeMoney(String userName, int money);
     	
     	public boolean findUser(String userName);
     }
     ```

   - com.systop.dao.impl.CardDaoImpl.java

     ```java
     package com.systop.dao.impl;
     
     import java.util.List;
     
     import org.springframework.jdbc.core.JdbcTemplate;
     
     import com.systop.dao.CardDao;
     
     public class CardDaoImpl implements CardDao {
     
         // spring框架自带的数据库操作对象
     	private JdbcTemplate jdbcTemplate;
     	
     	@Override
     	public int changeMoney(String userName, int money) {
     		String sql = "update card set money = money + ? where username = ?";
     		int i = this.jdbcTemplate.update(sql, money, userName);
     		return i;
     	}
     
     	@Override
     	public boolean findUser(String userName) {
     		String sql = "select * from card where username = ?";
     		List list = this.jdbcTemplate.queryForList(sql, userName);
     		if (list != null && list.size() > 0) {
     			return true;
     		} else {
     			return false;
     		}
     	}
     
     	public JdbcTemplate getJdbcTemplate() {
     		return jdbcTemplate;
     	}
     	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
     		this.jdbcTemplate = jdbcTemplate;
     	}
     }
     ```

   - com.systop.service.CardService.java

     ```java
     package com.systop.service;
     
     public interface CardService {
     	
     	public void transfer(String userOut, String userIn, int money);
     }
     ```

   - com.systop.service.impl.CardServiceImpl.java

     ```java
     package com.systop.service.impl;
     
     import org.springframework.transaction.annotation.Propagation;
     import org.springframework.transaction.annotation.Transactional;
     
     import com.systop.dao.CardDao;
     import com.systop.service.CardService;
     /**
      * 业务逻辑层
      * @author Administrator
      *
      */
     public class CardServiceImpl implements CardService {
     
     	private CardDao cardDao;
     	
     	//银行转账的方法
     	@Override
     	public void transfer(String userOut, String userIn, int money) {
     		try {
     			//检查用户
     			boolean result1 = cardDao.findUser(userOut);
     			boolean result2 = cardDao.findUser(userIn);
     			//判断结果
     			if (result1 == false) {
     				System.out.println("用户："+userOut+"不存在！");
     			} else if (result2 == false) {
     				System.out.println("用户："+userIn+"不存在！");
     			} else {
     				//真正的转账
     				cardDao.changeMoney(userOut, -money);
                     // 用1/0来作为错误测试事务
     				// int i = 1 / 0;
     				cardDao.changeMoney(userIn, money);
     				System.out.println("转账成功");
     			}
     		} catch (Exception e) {
     			e.printStackTrace();
                 // try catch将错误捕获，所以我们需要抛出一个新的错误
     			throw new RuntimeException("xxxxx");
     		}
     	}
     
     	public CardDao getCardDao() {
     		return this.cardDao;
     	}
     	public void setCardDao(CardDao cardDao) {
     		this.cardDao = cardDao;
     	}
     }
     ```

   - com.systop.test.Test.java

     ```java
     package com.systop.test;
     
     import org.springframework.context.ApplicationContext;
     import org.springframework.context.support.ClassPathXmlApplicationContext;
     
     import com.systop.service.CardService;
     
     public class Test {
     	public static void main(String[] args) {
     		ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
     		
     		CardService cardService = (CardService)app.getBean("cardService");
     		
     		cardService.transfer("Tom", "Jack", 1000);
     	}
     }
     ```

2. 基于Annotation注解方式的声明式注解

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
     	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
     	xmlns:aop="http://www.springframework.org/schema/aop"
     	xmlns:tx="http://www.springframework.org/schema/tx" 
     	xmlns:context="http://www.springframework.org/schema/context"
     	xsi:schemaLocation="http://www.springframework.org/schema/beans 
     	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
     	http://www.springframework.org/schema/aop
     	http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
     	http://www.springframework.org/schema/context
     	http://www.springframework.org/schema/context/spring-context-4.3.xsd
     	http://www.springframework.org/schema/tx
     	http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
     	
     	<!-- 1、数据源id固定名字不能写错dataSource -->
     	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
     		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
     		<property name="url" value="jdbc:mysql://localhost:3306/test" />
     		<property name="username" value="root" />
     		<property name="password" value="root" />
     	</bean>
     	
     	<!-- 2、Spring的JDBC模板 -->
     	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
     		<!-- 注入属性：数据源 -->
     		<property name="dataSource" ref="dataSource" />
     	</bean>
     	
     	<!-- 3 、 定义Bean对象CardDao -->
     	<bean id="cardDao" class="com.systop.dao.impl.CardDaoImpl">
     		<property name="jdbcTemplate" ref="jdbcTemplate"/>
     	</bean>
     	
     	<bean id="cardService" class="com.systop.service.impl.CardServiceImpl">
     		<property name="cardDao" ref="cardDao"/>
     	</bean>
     	
     	<!-- 事务管理器 -->
     	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     		<property name="dataSource" ref="dataSource" />
     	</bean>
     	
         <!-- 开启注解 -->
     	<tx:annotation-driven transaction-manager="transactionManager"/>
     </beans>
     ```

   - com.systop.service.impl.CardServiceImpl.java

     ```java
     package com.systop.service.impl;
     
     import org.springframework.transaction.annotation.Propagation;
     import org.springframework.transaction.annotation.Transactional;
     
     import com.systop.dao.CardDao;
     import com.systop.service.CardService;
     /**
      * 业务逻辑层
      * @author Administrator
      *
      */
     public class CardServiceImpl implements CardService {
     
     	private CardDao cardDao;
     	
     	//银行转账的方法
     	@Override
     	@Transactional(propagation=Propagation.REQUIRED,readOnly=false)
     	public void transfer(String userOut, String userIn, int money) {
     		try {
     			//检查用户
     			boolean result1 = cardDao.findUser(userOut);
     			boolean result2 = cardDao.findUser(userIn);
     			//判断结果
     			if (result1 == false) {
     				System.out.println("用户："+userOut+"不存在！");
     			} else if (result2 == false) {
     				System.out.println("用户："+userIn+"不存在！");
     			} else {
     				//真正的转账
     				cardDao.changeMoney(userOut, -money);
                     // 用1/0来作为错误测试事务
     				// int i = 1 / 0;
     				cardDao.changeMoney(userIn, money);
     				System.out.println("转账成功");
     			}
     		} catch (Exception e) {
     			e.printStackTrace();
                 // try catch将错误捕获，所以我们需要抛出一个新的错误
     			throw new RuntimeException("xxxxx");
     		}
     	}
     
     	public CardDao getCardDao() {
     		return this.cardDao;
     	}
     	public void setCardDao(CardDao cardDao) {
     		this.cardDao = cardDao;
     	}
     }
     ```

     > 使用注解方式，只需在配置文件中开启注解，在service层的类或方法上添加注解即可
     >
     > 使用注解方式减少了配置文件的代码量，增加了方法添加事务的灵活性，但增加注解配置次数，提高了代码量

## Spring MVC

> MVC(Model View Controller)是一种软件设计的框架模式，它采用模型(Model)-视图(View)-控制器(controller)的方法把业务逻辑、数据与界面显示分离。把众多的业务逻辑聚集到一个部件里面，当然这种比较官方的解释是不能让我们足够清晰的理解什么是MVC的。用通俗的话来讲，MVC的理念就是把数据处理、数据展示(界面)和程序/用户的交互三者分离开的一种编程模式。

优点：

- 结构松散，几乎可以在 Spring MVC 中使用各类视图
- 松耦合，各个模块分离
- 与 Spring 无缝集成
- 提供了一个前端控制器DispatcherServlet，开发人员无须额外开发控制器对象

### Spring入门应用

- 创建一个新的web项目，导入jar包

![1605172728875](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605172728875.png)

- web.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
  	id="WebApp_ID" version="3.1">
  	<display-name>ch</display-name>
      <!-- 配置前端过滤器 -->
  	<servlet>
  		<servlet-name>springmvc</servlet-name>
  		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  		<!-- 初始化springMVC时加载的配置文件 -->
  		<init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>
  				classpath:springmvc-config.xml
  			</param-value>
  		</init-param>
  		<!-- 项目启动就开始加载servlet -->
  		<load-on-startup>1</load-on-startup>
  	</servlet>
  	<servlet-mapping>
  		<servlet-name>springmvc</servlet-name>
  		<url-pattern>/</url-pattern>
  	</servlet-mapping>
  	<welcome-file-list>
  		<welcome-file>index.html</welcome-file>
  		<welcome-file>index.htm</welcome-file>
  		<welcome-file>index.jsp</welcome-file>
  		<welcome-file>default.html</welcome-file>
  		<welcome-file>default.htm</welcome-file>
  		<welcome-file>default.jsp</welcome-file>
  	</welcome-file-list>
  </web-app>
  ```

- springmvc-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  	xmlns:p="http://www.springframework.org/schema/p"
  	xmlns:context="http://www.springframework.org/schema/context"
  	xmlns:mvc="http://www.springframework.org/schema/mvc" 
  	xmlns:task="http://www.springframework.org/schema/task"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans 
          http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
          http://www.springframework.org/schema/context 
          http://www.springframework.org/schema/context/spring-context-4.2.xsd 
          http://www.springframework.org/schema/mvc 
          http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
          http://www.springframework.org/schema/task 
          http://www.springframework.org/schema/task/spring-task-4.2.xsd">
      
      <!-- 开启注解 -->
      <context:annotation-config />
      
      <!-- 扫描包 -->
      <context:component-scan base-package="com.systop.controller" />
      
      <!-- 视图解析器-->
  	<bean
  		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  		<!-- 配置从项目根目录到指定目录一端路径 ,建议指定浅一点的目录 -->
  		<property name="prefix" value="/WEB-INF/jsp/"></property>
  		<!-- 文件的后缀名 -->
  		<property name="suffix" value=".jsp"></property>
  	</bean>
  	
  </beans>
  ```

- com.systop.pojo.User.java

  ```java
  package com.systop.pojo;
  
  import org.springframework.stereotype.Component;
  
  @Component
  public class User {
  	private String username;
  	private String password;
  	
  	public User() {
  	}
  	
  	public String getUsername() {
  		return username;
  	}
  	public void setUsername(String username) {
  		this.username = username;
  	}
  	public String getPassword() {
  		return password;
  	}
  	public void setPassword(String password) {
  		this.password = password;
  	}
  }
  ```

- com.systop.controller.TestController.java

  ```java
  package com.systop.controller;
  
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpSession;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.PathVariable;
  import org.springframework.web.bind.annotation.PostMapping;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;
  import org.springframework.web.servlet.ModelAndView;
  
  import com.systop.pojo.User;
  
  @Controller
  public class TestController {
  	
      // 在浏览器输入地址“http://localhost:8080/ch/login”,前端控制器找到这个方法并执行
  	@GetMapping("/login")
      // @RequestMapping(value="/login",method=RequestMethod.GET)
  	public ModelAndView login(ModelAndView mav) {
          // 设置视图名称
  		mav.setViewName("login");
  		return mav;
          // 前端控制器接收返回值，判断返回类型，通过视图解析器拼接路径 “/WEB-INF/jsp/login.jsp”
  	}
  	
      // 通过post表单提交，转到“/ch/abc”,执行此方法，自动向对象中设置用户名，密码
      // PostMapping("/abc")
  	@RequestMapping(value="/abc",method=RequestMethod.POST)
  	public ModelAndView login1(User user) {
  		System.out.println(user.getUsername());
  		System.out.println(user.getPassword());
  		return null;
  	}
      
      // 在浏览器输入地址“http://localhost:8080/ch/user/1”，这是在路径中提交数据
      @RequestMapping(value = "/user/{id}", method = RequestMethod.GET)
  	public String selectUserById(@PathVariable String id) {
  		System.out.println(id);	// 输出1
  		return null;
  	}
  
  	@RequestMapping("/getuser")
      // 不需要new对象，设置参数，前端控制器会自动传入
  	public ModelAndView getUser(HttpSession session,HttpServletRequest request,ModelAndView mav) {
  		System.out.println(session);
  		System.out.println(mav);
  		System.out.println(request);
  		return null;
  	}
  
  	@RequestMapping("/update")
  	public String update(int a) {
  		System.out.println(a);
  		return "redirect:getuser";
          // 重定向 /ch/getuser
  	}
  
  	@RequestMapping("/update1")
  	public String update1() {
  		return "forward:getuser";
          // 转发 /ch/getuser
  	}
  }
  ```

- /WEB-INF/jsp/login.jsp

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  	<form action="/ch/abc" method="post">
  		<input type="text" name="username"/><br/>
  		<input type="password" name="password"/><br/>
  		<input type="submit" value="登录"/>
  	</form>
  </body>
  </html>
  ```

- /WEB-INF/jsp/index.jsp

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  	首页
  </body>
  </html>
  ```

- 测试：

  1. 在浏览器输入地址“http://localhost:8080/ch/login”
  2. 输入用户名，密码提交，查看控制台
  3. 在浏览器输入地址“http://localhost:8080/ch/user/1”，查看控制台

> 注：不需要new对象，设置参数，前端控制器会自动传入

### Spring MVC 的工作流程

![1605421604466](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605421604466.png)

1. 用户向服务器发送请求时，请求会被Spring 前端控制器DispatcherServlet所拦截
2. DispatcherServlet拦截到请求后，会调用HandlerMapping处理器映射器
3. 处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet
4. DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
5. HeadlerAdapter会调用并执行Headler（处理器）这里的处理器指的就是程序中编写的Controller类，也被称之为后端控制器，页面控制器
6. Handler执行完成后，会返回一个ModelAndView对象，该对象中会包含视图名或包模型和视图名
7. HandlerAdapter将Handler执行结果ModelAndView对象返回给DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后,会向DispatcherServlet中返回具体的View（视图）
10. DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）
11. 视图渲染结果会返回给客户端浏览器显示

> ​	DispatcherServlet在代码编译时，将所有的controller类和类中的方法以键值对【uri，类和方法】的方式存储起来，当用户通过客户端向服务器发送请求，DispatcherServlet会拦截请求，通过请求路径中的uri找到controller类和方法，通过映射的方式获取方法的参数类型和参数名称，并根据参数名称匹配请求中的数据，经过格式转换，并通过方法对象Method的invoke(Object obj, Object... args) 方法，执行uri指向的方法，DispatcherServlet根据返回结果来确定相应客户端的是页面还是数据，若是页面则根据结果渲染页面并展示，若是数据则将数据转换为json返回给客户端。

拓展：

**一、URI**

**<1>什么是URI**

​	URI 在电脑术语中，统一资源标识符（Uniform Resource Identifier，URI)是一个用于标识某一互联网资源名称的字符串。 该种标识允许用户对任何（包括本地和互联网）的资源通过特定的协议进行交互操作。URI由包括确定语法和相关协议的方案所定义。

​	Web上可用的每种资源  HTML文档、图像、视频片段、程序等 - 由一个通用资源标识符（Uniform Resource Identifier, 简称"URI"）进行定位。

**<2>URI的结构组成**

URI通常由三部分组成：

- 资源的命名机制；
- 存放资源的主机名；
- 资源自身的名称。

> 注意：这只是一般URI资源的命名方式，只要是可以唯一标识资源的都被称为URI，上面三条合在一起是URI的充分不必要条件

**<3>URI举例**

如：https://blog.csdn.net/qq_32595453/article/details/79516787

我们可以这样解释它：

1. 这是一个可以通过https协议访问的资源，
2. 位于主机 blog.csdn.net上，
3. 通过“/qq_32595453/article/details/79516787”可以对该资源进行唯一标识（注意，这个不一定是完整的路径）

> 注意：以上三点只不过是对实例的解释，以上三点并不是URI的必要条件，URI只是一种概念，怎样实现无所谓，只要它唯一标识一个资源就可以了。

**二、URL**

URL是URI的一个子集。它是Uniform Resource Locator的缩写，译为“统一资源定位 符”。

通俗地说，URL是Internet上描述信息资源的字符串，主要用在各种客户程序和服务器程序上。

采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL是URI概念的一种实现方式。

URL的一般格式为(带方括号[]的为可选项)：

`protocol :// hostname[:port] / path / [;parameters][?query]#fragment`

URL的格式由三部分组成： 

1. 第一部分是协议(或称为服务方式)。

2. 第二部分是存有该资源的主机IP地址(有时也包括端口号)。

3. 第三部分是主机资源的具体地址，如目录和文件名等。

   ​	第一部分和第二部分用“://”符号隔开，

   ​	第二部分和第三部分用“/”符号隔开。

   ​	第一部分和第二部分是不可缺少的，第三部分有时可以省略。 

**三、URI和URL之间的区别**

从上面的例子来看，你可能觉得URI和URL可能是相同的概念，其实并不是，URI和URL都定义了资源是什么，但URL还定义了该如何访问资源。URL是一种具体的URI，它是URI的一个子集，它不仅唯一标识资源，而且还提供了定位该资源的信息。URI 是一种语义上的抽象概念，可以是绝对的，也可以是相对的，而URL则必须提供足够的信息来定位，是绝对的。

### Spring MVC 的核心类和注解

**DispatcherServlet**

> DispatcherServlet全名是 `org.springframework.web.servlet.DispatcherServlet`,它在程序中充当着前端控制器的角色。在使用时，只需将它配置在项目的web.xml文件中即可

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	id="WebApp_ID" version="3.1">
	<display-name>ch</display-name>
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 初始化springMVC时加载的配置文件 -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>
				classpath:springmvc-config.xml
			</param-value>
		</init-param>
		<!-- 项目启动就开始加载servlet -->
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

**Controller注解类型**

> `org.springframework.stereotype.Controller`注解类型用于指示Spring类的实例是一个控制器，其注解形式为`@Controller`。该注解在使用时不需要在实现Controller接口，只需要将`@Controller`注解加入到控制器类上，然后通过Spring的扫描机制找到标注了该注解的控制器即可。

**RequestMapping 注解**

> Spring 通过 `@Controller` 注解找到相应的控制器类后，还要知道控制器内部对每个请求的是如何处理的，这就用到`org.springframework.web.bind.annotation.RequestMapping`注解类型用于映射一个请求或一个方法，其注解形式为`@RequestMapping`，可以使用该注解标注在一个方法或一个类上。

​	**属性**

​		**value：**默认属性，用于映射一种请求和一种方法，在类和方法上的value拼接就是方法的uri

​		**method：**用于指定该方法处理哪种请求方式，请求方式包括GET、POST、HEAD、OPTIONS、PUT、PATCH、DELETE和TRACE，例：method=RequestMethod.GET，多种请求方式用{}写成数组的形式

​	**组合注解**

​		用于替代单个请求方式的@RequestMapping

​		@GetMapping：匹配GET方式的请求

​		@PostMapping：匹配POST方式的请求

​		@PutMapping：匹配PUT方式的请求

​		@DeleteMapping：匹配DELETE方式的请求

​		@PatchMapping：匹配PATCH方式的请求

### Spring MVC 的参数类型和返回类型

- **参数类型**

  1. **javax.servlet.http.HttpServletRequest**
  2. **javax.servlet.http.HttpServletResponse**
  3. **java.servlet.http.HttpSession**
  4. **java.io.InputStream/java.io.Reader**
  5. **java.io.OutputStream/java.io.Writer**
  6. **@RequestPerem、@RequestBody**
  7. **java.util.Map**
  8. **org.springframework.web.servlet.ModelAndView**
  9. **pojo类**

- **返回类型**

  1. **org.springframework.web.servlet.ModelAndView**
  2. **java.lang.String**
  3. **java.util.Map**
  4. **void**
  5. **pojo类**
  6. **java.util.List**

- **参数类型注意事项**

  1. 不需要在方法中new任何对象，spring框架会自动调用参数类的无参构造生成对象传入，若是pojo类型，spring框架还会判断请求数据中是否有与pojo类属性相同的数据名，若有则自动填入
  2. 在参数前加加入@RequestPerem(value="")注解，意为将请求数据中的与value的值一致的数据传入该参数
  3. 在参数前加加入@RequestBody注解，意为该参数为json数据传入

- **返回类型注意事项**

  1. ModelAndView返回类型：
     - ModelAndView的addObject(String attributeName, Object attributeValue)方法相当于HttpServletRequest的setAttribute(String name, Object o)方法，它同样能在jsp页面通过${}显示
     - ModelAndView的setViewName(String viewName)方法传入的是下一步要跳转的页面的路径
  2. String返回类型有三种处理方式：
     - 返回下一步要跳转的页面的路径
     - 重定向请求，以"redirect:"开头，后面接路径
     - 转发请求，以"forward:"开头，后面接路径
  3. 在方法加@responseBody注解返回类型自动转换为json数据，这种方式需要导入json的转换包

- 视图解析器

  我们的springmvc的入门程序中，String返回类型的返回值和ModelAndView返回类型的viewName属性的值都不是全部的路径，那是因为我们在springmvc-config.xml中配置了视图解析器

  ```xml
  <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <!-- 配置从项目根目录到指定目录一端路径 ,建议指定浅一点的目录 -->
      <property name="prefix" value="/WEB-INF/jsp/"></property>
      <!-- 文件的后缀名 -->
      <property name="suffix" value=".jsp"></property>
  </bean>
  ```

  它会自动帮我们拼接路径，即真实路径=前缀+返回路径+后缀

### 静态资源解放

> 在配置了DispatcherServlet之后所有的资源都必须经过它，这会导致我们的静态资源html、css、js、img等等都会被拦截，导致使用非常不方便

解放方式：在springmvc-config.xml中配置

```xml
<!-- location：定位本地静态资源文件路径，具体到某个文件夹 -->
<!-- mapping：匹配静态资源全路径，可以使用通配符，全部文件是"/**" -->
<mvc:resources location="/" mapping="/*.html"/>
```

### 自定义数据转换

> 在一般情况下，使用基本数据类型和pojo类型的参数数据已经能够满足需求，然而有些特殊类型的参数是无法在后台进行直接转换的，例如时间类型就需要开发者自定义转换器

**Converter**

定义Converter类要实现`org.springframework.core.convert.converter.Converter`接口，该接口代码如下

```java
public interface Converter<S, T> {
    T convert(S source);
}
```

泛型中的S表示原类型，T标识目标类型，而convert(S source)表示接口中的方法。

com.systop.convert.DateConvert

```java
package com.systop.convert;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.core.convert.converter.Converter;

public class DateConvert implements Converter<String, Date> {

	private String datePattern = "yyyy-MM-dd HH:mm:ss";
	@Override
	public Date convert(String source) {
		SimpleDateFormat sdf = new SimpleDateFormat(datePattern);
		Date day = null;
		try {
			day = sdf.parse(source);
		} catch (ParseException e) {
			throw new IllegalArgumentException("ÎÞÐ§ÈÕÆÚ");
		}
		return day;
	}

}
```

springmvc-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc" 
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
        http://www.springframework.org/schema/task 
        http://www.springframework.org/schema/task/spring-task-4.2.xsd">

	<!-- 注解扫包！ -->
	<context:component-scan base-package="com.systop.controller" />
	
	<!-- 静态资源 -->
	<mvc:resources location="/" mapping="/*.html"/>
	<mvc:resources location="/js/" mapping="/js/**"/>
	
	<!-- 试图解析器 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 配置从项目根目录到指定目录一端路径 ,建议指定浅一点的目录 -->
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<!-- 文件的后缀名 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 手动装配自定义类型的转换器 -->
	<mvc:annotation-driven conversion-service="conversionService" />
	<!-- 配置日期转换器 -->
	<bean id="conversionService" 
		class="org.springframework.context.support.ConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<bean class="com.systop.convert.DateConvert"/>
			</set>
		</property>
	</bean>
</beans>
```

在配置文件中配置自定义的转换器即可

### 拦截器

> Spring MVC 中的拦截器（Interceptor）类似于Servlet中的过滤器（Filter），它主要用于拦截用户请求并做相应的处理。例如通过拦截器可以进行权限验证、记录请求信息的日志、判断用户是否登录等等

定义拦截器有两种方式：

1. 实现HandlerInterceptor接口，或继承HandlerInterceptor接口的实现类
2. 实现WebRequestInterceptor接口，或继承WebRequestInterceptor接口的实现类

以实现HandlerInterceptor接口为例

```java
package com.systop.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class CostomInterceptor implements HandlerInterceptor {

	@Override
	public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2) throws Exception {
		// TODO Auto-generated method stub
		return false;
	}

	@Override
	public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, ModelAndView arg3)
			throws Exception {
		// TODO Auto-generated method stub
		
	}
    
	@Override
	public void afterCompletion(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, Exception arg3)
			throws Exception {
		// TODO Auto-generated method stub
		
	}

}
```

**preHandle()方法：**该方法会在控制器方法执行之前执行，其返回值表示是否中断后续的操作。当其返回值为true时，表示继续向下执行；当其返回值为false时，会中断后续的所有操作（包括调用下一个拦截器和控制器类中的方法执行等），可以通过此方法进行数据验证，权限审查等。

**postHandle()方法：**该方法会在控制器方法调用之后，且解析视图之前执行。可以通过此方法对请求域中的模型和视图做出进一步的修改。

**afterCompletion()方法：**该方法会在整个请求完成，即视图渲染结束之后执行。可以通过此方法实现一些资源清理、记录日志信息等工作。

在springmvc-config.xml中添加

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <!-- 配置拦截器作用路径 -->
        <mvc:mapping path="/**"/>
        <!--  配置放行路径  -->
        <mvc:exclude-mapping path=""/>
        <!-- 配置拦截器  -->
        <bean class="com.systop.interceptor.CostomInterceptor" />
    </mvc:interceptor>
</mvc:interceptors>
```

> 注：每个mvc:interceptor标签下的配置必须按照mvc:mapping -> mvc:exclude-mapping -> bean的顺序配置，不然就会报错

**拦截器的执行顺序**

<一> 单个拦截器

preHeadle()  --return true-->  controller  --->  postHandle()  --->  DispatcherServlet  --->  afterCompletion()

> 先执行拦截器中的praHeadle()的方法，返回值为true时，执行controller类中的逻辑方法，在之后指向拦截器的postHeadle()方法，再到DispatcherServlet的数据处理，最后执行拦截器afterCompletion()方法，结束

**<二>多个拦截器**

preHeadle1()  --->  preHeadle2()  --->  controller  --->  postHandle2()  --->  postHandle1()  --->  DispatcherServlet  --->  afterCompletion2()  --->  afterCompletion1()

> 多个拦截器按照在springmvc-config.xml中配置的先后顺序，先从前到后执行preHeadle()方法，全部返回true之后，执行controller类中的逻辑方法，在从后到前执行postHandle()方法，在通过DispatcherServlet处理数据，最后在从后到前执行afterCompletion()方法，可以参考数据结构中的先入栈后出栈逻辑

### 文件的上传和下载

> 文件的上传与下载，本质上就是二进制流的传输

**文件的上传**需要有一个文件上传的表单：

```html
<from action="" method="post" enctype="multipart/form-data">
	<input type="file" name="filename" multiple="multiple"/>
    <input type="submit" value="文件上传"/>
</from>
```

​	**当客户端 from 表单的 enctype 属性为 multipart/form-data 时，浏览器就会采用二进制流的方式来处理表单数据，服务器端就会对文件上传的请求进行解析处理。**

​	Spring MVC 为文件上传提供了直接的支持，这种支持是通过MultipartResolver(多部件解析器)对象实现的。而我们只需要在springmvc-config.xml配置文件中定义它的Bean即可

```xml
<!-- 上传解析器 -->
<bean id="multipartResolver"
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 设置编码格式 -->
    <property name="defaultEncoding" value="UTF-8"/>
    <!-- 设置最大上传长度，单位是字节(Btye) -->
    <property name="maxUploadSize" value="5242880"/><!-- 5M -->
    <!-- 设置最大缓存长度，单位是字节(Btye) -->
    <property name="maxInMemorySize" value="1048576"/><!-- 1M -->
    <!-- 设置推迟文件解析，以便在Controller中捕获文件大小异常 -->
    <property name="resolveLazily" value="true"/><!-- 默认为false -->
</bean>
```

**文件的下载**就是将文件服务器上的文件下载到本机上

在客户端页面上我们需要有一个文件下载的超链接，该链接的href属性指定后台文件的方法以及文件名

```html
<a href="${pageContext.request.contextPath}/download?filename=1.jpg">文件下载</a>
```

编写一个简单的上传下载系统

- jar包

  ant-1.9.6.jar

  ant-launcher-1.9.6.jar

  asm-5.1.jar

  aspectjweaver-1.8.10.jar

  cglib-3.2.4.jar

  commons-dbcp2-2.1.1.jar

  commons-fileupload-1.3.2.jar

  commons-io-2.5.jar

- web.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
    <display-name>ch13</display-name>
     <!-- 解决中文乱码问题 -->
    <filter>
    	<filter-name>encoding</filter-name>
    	<filter-class>
    		org.springframework.web.filter.CharacterEncodingFilter
    	</filter-class>
    	<init-param>
    		<param-name>encoding</param-name>
    		<param-value>UTF-8</param-value>
    	</init-param>
    </filter>
    <filter-mapping>
    	<filter-name>encoding</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- servlet -->
    <servlet>
    	<servlet-name>springmvc</servlet-name>
    	<servlet-class>
    		org.springframework.web.servlet.DispatcherServlet
    	</servlet-class>
    	<!-- 初始化参数 -->
    	<init-param>
    		<param-name>contextConfigLocation</param-name>
    		<param-value>classpath:springmvc-config.xml</param-value>
    	</init-param>
    	<!-- 项目加载就要启动 -->
    	<load-on-startup>1</load-on-startup>
    </servlet>
    <!-- servlet映射 -->
    <servlet-mapping>
    	<servlet-name>springmvc</servlet-name>
    	<url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.htm</welcome-file>
      <welcome-file>index.jsp</welcome-file>
      <welcome-file>default.html</welcome-file>
      <welcome-file>default.htm</welcome-file>
      <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>
  </web-app>
  ```

- spingmvc-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  	xmlns:p="http://www.springframework.org/schema/p"
  	xmlns:context="http://www.springframework.org/schema/context"
  	xmlns:mvc="http://www.springframework.org/schema/mvc" 
  	xmlns:task="http://www.springframework.org/schema/task"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans 
          http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
          http://www.springframework.org/schema/context 
          http://www.springframework.org/schema/context/spring-context-4.2.xsd 
          http://www.springframework.org/schema/mvc 
          http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
          http://www.springframework.org/schema/task 
          http://www.springframework.org/schema/task/spring-task-4.2.xsd">
  
  	<!-- 注解扫包！ -->
  	<context:component-scan base-package="com.systop.controller" />
  	
  	<!-- 静态资源 -->
  	<mvc:resources location="/js/" mapping="/js/**"/>
  	
  	<!-- 试图解析器 -->
  	<bean
  		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  		<!-- 配置从项目根目录到指定目录一端路径 ,建议指定浅一点的目录 -->
  		<property name="prefix" value="/WEB-INF/jsp/"></property>
  		<!-- 文件的后缀名 -->
  		<property name="suffix" value=".jsp"></property>
  	</bean>
  	
  	<!-- 手动装配自定义类型的转换器 -->
  	<mvc:annotation-driven/>
  	
  	<bean id="multipartResolver"
  		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  		<property name="defaultEncoding" value="UTF-8"/>
  		<property name="maxUploadSize" value="1024000"></property>
  	</bean>
      
  </beans>
  ```

- /WEB-INF/jsp/upload.jsp

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <!DOCTYPE HTML>
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript">
  	function check(){
  		var name = document.getElementById("name").value;
  		var filename = document.getElementById("uploadfile").value;
  		if(name == ""){
  			alert("请填写上传人");
  			return false;
  		}
  		if(filename == "" || filename.length == 0){
  			alert("请选择文件");
  			return false;
  		}
  		return true;
  	}
  </script>
  </head>
  <body>
  	<form action="${pageContext.request.contextPath }/upload" enctype="multipart/form-data" method="post" onsubmit="check()">
  		上传人<input type="text" id="name" name="name"/><br/>
  		上传文件<input type="file" name="uploadfile" id="uploadfile"/>
  		<!-- <input type="file" name="uploadfile" id="uploadfile" multiple="multiple"/> -->
  		<input type="submit" value="上传" />
  	</form>
  </body>
  </html>
  ```

- com.systop.controller.FileController.java

  ```java
  package com.systop.controller;
  
  import java.io.File;
  import java.net.URLEncoder;
  import java.util.UUID;
  
  import javax.servlet.http.HttpServletRequest;
  
  import org.apache.commons.io.FileUtils;
  import org.springframework.http.HttpHeaders;
  import org.springframework.http.HttpStatus;
  import org.springframework.http.MediaType;
  import org.springframework.http.ResponseEntity;
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.PostMapping;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;
  import org.springframework.web.bind.annotation.RequestParam;
  import org.springframework.web.multipart.MultipartFile;
  
  @Controller
  public class FileController {
  
  	@GetMapping("/upload")
  	public String toUpload() {
  		return "upload";
  	}
  	
  	@PostMapping("/upload")
  	public String upload(@RequestParam("name") String name, @RequestParam("uploadfile") MultipartFile uploadfile,HttpServletRequest request) {
  		//文件的原始名称
  		String originalFilename = uploadfile.getOriginalFilename();
  		//查找项目中是否有upload路径
  		String path = request.getServletContext().getRealPath("/upload");
  		//新建file文件
  		File filepath = new File(path);
  		if(!filepath.exists()) {
  			filepath.mkdirs();
  		}
  		//生成新的名字
  		String newFileName = name+"_"+UUID.randomUUID() + "_"+originalFilename;
  		try {
  			//使用MultipartFile类transferTo方法上传参数是个文件filepath+newFileName==/upload/xxx_1.txt
  			uploadfile.transferTo(new File(filepath+"/"+newFileName));
  		} catch (Exception e) {
  			// TODO 自动生成的 catch 块
  			e.printStackTrace();
  			return "index";
  		}
  		return "index";
  	}
  	
  	@GetMapping("/download")
  	public String download() {
  		return "download";
  	}
  	
  	@RequestMapping(value="/todownload",method=RequestMethod.GET)
  	public ResponseEntity<byte []> fileDownload(HttpServletRequest request,String filename) throws Exception {
  		//找到文件的路径
  		String path = request.getServletContext().getRealPath("/upload");
  		//根据文件的路径和文件名找到该文件
  		File file = new File(path + File.separator + filename);
  		//解决中文乱码问题
  		filename = this.getFilename(request, filename);
  		//设置头信息
  		HttpHeaders headers = new HttpHeaders();
  		//通知浏览器以下载的方式打开文件
  		headers.setContentDispositionFormData("attachment", filename);
  		//定义一流方式下载文件
  		headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
  		//使用spring mvc框架提供的ResponseEntity返回下载数据，第一个参数是字节，第二个参数头信息，第三个参数是状态
  		return new ResponseEntity<byte []>(FileUtils.readFileToByteArray(file),headers,HttpStatus.OK);
  	}
  	
  	
  	public String getFilename(HttpServletRequest request, String fileName) throws Exception {
  		String [] IEBrowserKeyWords = {"MSIE","Trident","Edge"};
  		String userAgent = request.getHeader("User-Agent");
  		for (String keywords : IEBrowserKeyWords) {
  			if(userAgent.contains(keywords)) {
  				return URLEncoder.encode(fileName,"UTF-8");
  			}
  		}
  		//
  		return new String(fileName.getBytes("UTF-8"),"ISO-8859-1");
  	}
  }
  ```

- /WEB-INF/jsp/download.jsp

  ```jsp
  <%@ page language="java" contentType="text/html; charset=utf-8"
      pageEncoding="utf-8"%>
  <%@ page import="java.net.URLEncoder" %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <title>Insert title here</title>
  </head>
  <body>
  	<a href="${pageContext.request.contextPath }/todownload?filename=<%=URLEncoder.encode("Spring框架技术_15.docx","UTF-8")%>">下载中文文件</a>
  	<a href="${pageContext.request.contextPath }/todownload?filename=1.txt">下载英文文件</a>
  	<a href="${pageContext.request.contextPath }/todownload?filename=<%=URLEncoder.encode("购物车.jpg","UTF-8")%>">下载中文图片</a>
  	<a href="${pageContext.request.contextPath }/file/car.jpg">下载英文图片</a>
  </body>
  </html>
  ```

  > 需要注意的是，上传的文件不是在项目中的目录下，而是在项目的发布路径中

## SSM 框架整合

> SpringMVC是Spring的子模块，不存在整合问题，因此SSM框架整合只涉及到Spring与MyBatis的整合，以及SpringMVC与MyBatis的整合

1. 准备所需jar包

![1605523953128](D:/Administrator/Desktop/document/%E7%AC%94%E8%AE%B0/JavaRecord.assets/1605523953128.png)

1. 或maven项目的pom.xml文件

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   	<modelVersion>4.0.0</modelVersion>
   	<groupId>cn.wisdsoft</groupId>
   	<artifactId>maven_ssm</artifactId>
   	<version>0.0.1-SNAPSHOT</version>
   	<packaging>war</packaging>
   
   	<properties>
   		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
   		<spring.version>4.3.6.RELEASE</spring.version>
   		<mybatis.version>3.4.1</mybatis.version>
   		<spring-mybatis.version>1.3.1</spring-mybatis.version>
   		<aspectj.version>1.9.1</aspectj.version>
   		<aopalliance.version>1.0</aopalliance.version>
   	</properties>
   
   	<dependencies>
   		<!-- spring核心包 -->
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-context</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-jdbc</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   
   		<!-- spring web核心包 -->
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-web</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<!-- spring MVC核心包 -->
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-webmvc</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<!-- spring 事务 -->
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-tx</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<!-- 事务切面包 -->
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-aspects</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.aspectj</groupId>
   			<artifactId>aspectjweaver</artifactId>
   			<version>${aspectj.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>aopalliance</groupId>
   			<artifactId>aopalliance</artifactId>
   			<version>${aopalliance.version}</version>
   		</dependency>
   		<!-- 日志 -->
   		<dependency>
   			<groupId>log4j</groupId>
   			<artifactId>log4j</artifactId>
   			<version>1.2.17</version>
   		</dependency>
   		<dependency>
   			<groupId>org.apache.logging.log4j</groupId>
   			<artifactId>log4j-api</artifactId>
   			<version>2.3</version>
   		</dependency>
   		<dependency>
   			<groupId>org.apache.logging.log4j</groupId>
   			<artifactId>log4j-core</artifactId>
   			<version>2.3</version>
   		</dependency>
   		<dependency>
   			<groupId>org.slf4j</groupId>
   			<artifactId>slf4j-api</artifactId>
   			<version>1.7.22</version>
   		</dependency>
   		<dependency>
   			<groupId>org.slf4j</groupId>
   			<artifactId>slf4j-log4j12</artifactId>
   			<version>1.7.22</version>
   			<scope>test</scope>
   		</dependency>
   		<!-- 连接池 -->
   		<dependency>
   			<groupId>org.apache.commons</groupId>
   			<artifactId>commons-dbcp2</artifactId>
   			<version>2.1.1</version>
   		</dependency>
   		<dependency>
   			<groupId>org.apache.commons</groupId>
   			<artifactId>commons-pool2</artifactId>
   			<version>2.4.2</version>
   		</dependency>
   		<!-- mysql驱动 -->
   		<dependency>
   			<groupId>mysql</groupId>
   			<artifactId>mysql-connector-java</artifactId>
   			<version>5.1.40</version>
   		</dependency>
   		<!-- mybatis核心包 -->
   		<dependency>
   			<groupId>org.mybatis</groupId>
   			<artifactId>mybatis</artifactId>
   			<version>${mybatis.version}</version>
   		</dependency>
   		<!-- mybatis-spring整合包 -->
   		<dependency>
   			<groupId>org.mybatis</groupId>
   			<artifactId>mybatis-spring</artifactId>
   			<version>${spring-mybatis.version}</version>
   		</dependency>
   		<!-- 其他 -->
   		<dependency>
   			<groupId>ognl</groupId>
   			<artifactId>ognl</artifactId>
   			<version>3.1.12</version>
   		</dependency>
   		<dependency>
   			<groupId>cglib</groupId>
   			<artifactId>cglib</artifactId>
   			<version>3.2.4</version>
   		</dependency>
   		<dependency>
   			<groupId>org.ow2.asm</groupId>
   			<artifactId>asm</artifactId>
   			<version>5.1</version>
   		</dependency>
   		<dependency>
   			<groupId>javax.servlet</groupId>
   			<artifactId>servlet-api</artifactId>
   			<version>2.5</version>
   			<scope>provided</scope>
   		</dependency>
   		<dependency>
   			<groupId>javax.servlet.jsp</groupId>
   			<artifactId>jsp-api</artifactId>
   			<version>2.2</version>
   			<scope>provided</scope>
   		</dependency>
   	</dependencies>
   	<build>
   		<plugins>
   			<plugin>
   				<groupId>org.apache.tomcat.maven</groupId>
   				<artifactId>tomcat7-maven-plugin</artifactId>
   				<version>2.2</version>
   				<configuration>
   					<port>8081</port>
   					<path>/</path>
   				</configuration>
   			</plugin>
   		</plugins>
   	</build>
   </project>
   ```

2. 创建一个新的源文件夹resources，在里面创建配置文件

   - db.properties

     ```properties
     # 数据库链接
     jdbc.driver=com.mysql.jdbc.Driver
     # 数据库地址，解决中文乱码
     jdbc.url=jdbc:mysql://localhost:3306/swgds?characterEncoding=UTF-8
     # 用户名
     jdbc.username=root
     # 密码
     jdbc.password=root
     # 最大连接数
     jdbc.maxTotal=30
     # 最大空闲连接数
     jdbc.maxIdle=10
     # 初始连接数
     jdbc.initialSize=5
     ```

   - log4j.properties

     ```properties
     # Global logging configuration
     log4j.rootLogger=DEBUG, stdout
     # MyBatis logging configuration...
     log4j.logger.cn.wisdsoft.swg=DEBUG
     # Console output...
     log4j.appender.stdout=org.apache.log4j.ConsoleAppender
     log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
     log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
     ```

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
     	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     	xmlns:aop="http://www.springframework.org/schema/aop"
     	xmlns:tx="http://www.springframework.org/schema/tx"
     	xmlns:context="http://www.springframework.org/schema/context"
     	xsi:schemaLocation="http://www.springframework.org/schema/beans 
     	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
     	http://www.springframework.org/schema/aop
     	http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
     	http://www.springframework.org/schema/context
     	http://www.springframework.org/schema/context/spring-context-4.3.xsd
     	http://www.springframework.org/schema/tx
     	http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
     	
         <!-- 开启注解 -->
     	<context:annotation-config />
     	
         <!-- 扫包 -->
     	<context:component-scan base-package="cn.wisdsoft.swg.service" />
     	
     	<!-- 读取db.properties -->
     	<context:property-placeholder location="classpath:db.properties"/>
     	
     	<!-- 数据源 -->
     	<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
     		<!-- 数据库驱动  -->
     		<property name="driverClassName" value="${jdbc.driver}" />
     		<!-- 数据库连接地址  -->
     		<property name="url" value="${jdbc.url}" />
     		<!-- 数据库连接的用户名  -->
     		<property name="username" value="${jdbc.username}" />
     		<!-- 数据库连接的密码 -->
     		<property name="password" value="${jdbc.password}" />
     		<!-- 最大连接数 -->
     		<property name="maxTotal" value="${jdbc.maxTotal}" />
     		<!-- 最大空闲数 -->
     		<property name="maxIdle" value="${jdbc.maxIdle}" />
     		<!-- 初始化连接数 -->
     		<property name="initialSize" value="${jdbc.initialSize}" />
     	</bean>
     	
     	<!-- 事务管理器 -->
     	<bean id="transactionManager" 
     		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     		<property name="dataSource" ref="dataSource" />
     	</bean>
     	
     	<!-- 注解方式配置事务 -->
     	<tx:annotation-driven transaction-manager="transactionManager"/>
     	
     	<!-- 配置mybatis工厂 -->
     	<bean id="sqlSessionFactory" 
     		class="org.mybatis.spring.SqlSessionFactoryBean">
     		<property name="dataSource" ref="dataSource" />
     		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
     	</bean>
     	
     	<!--配置加载Mapper代理-->
         <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
         	<!-- 扫描mapper所在的包 -->
             <property name="basePackage" value="cn.wisdsoft.swg.mapper"/>
         </bean>
     </beans>
     ```

   - mybatis-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
         "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
     	<!-- 起别名 -->
     	<typeAliases>
     		<package name="cn.wisdsoft.swg.pojo"/>
     	</typeAliases>
     </configuration>
     ```

   - springmvc-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
     	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
     	xmlns:context="http://www.springframework.org/schema/context"
     	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:task="http://www.springframework.org/schema/task"
     	xsi:schemaLocation="http://www.springframework.org/schema/beans 
             http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
             http://www.springframework.org/schema/context 
             http://www.springframework.org/schema/context/spring-context-4.2.xsd 
             http://www.springframework.org/schema/mvc 
             http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
             http://www.springframework.org/schema/task 
             http://www.springframework.org/schema/task/spring-task-4.2.xsd">
     
     	<!-- 扫描内容 -->
     	<context:component-scan base-package="cn.wisdsoft.swg.controller" />
     	<context:component-scan base-package="cn.wisdsoft.swg.api" />
     	<!-- 开启注解配置 -->
     	<mvc:annotation-driven />
     
     	<!-- 配置静态资源的访问映射，此配置中的文件，将不被前端控制器拦截 -->
     	<mvc:resources location="/js/" mapping="/js/**" />
     	<mvc:resources location="/images/" mapping="/images/**" />
     	<mvc:resources location="/css/" mapping="/css/**" />
     	<mvc:resources location="/html/" mapping="/html/**" />
     
     	<!-- 视图解析器 -->
     	<bean id="viewResolver"
     		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
     		<!-- 前缀 -->
     		<property name="prefix" value="/WEB-INF/jsp/" />
     		<!-- 后缀 -->
     		<property name="suffix" value=".jsp" />
     	</bean>
     </beans>
     ```

   - web.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
     	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
     	id="WebApp_ID" version="3.1">
     	<display-name>swg</display-name>
     	<!-- 编码集 -->
     	<filter>
     		<filter-name>encoding</filter-name>
     		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
     		<init-param>
     			<param-name>encoding</param-name>
     			<param-value>UTF-8</param-value>
     		</init-param>
     	</filter>
     	<filter-mapping>
     		<filter-name>encoding</filter-name>
     		<url-pattern>/*</url-pattern>
     	</filter-mapping>
     	<!-- spring 监听器 -->
     	<context-param>
     		<param-name>contextConfigLocation</param-name>
     		<param-value>classpath:applicationContext.xml</param-value>
     	</context-param>
     	<listener>
     		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
     	</listener>
     	<!-- spring mvc -->
     	<servlet>
     		<servlet-name>springmvc</servlet-name>
     		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
     		<init-param>
     			<param-name>contextConfigLocation</param-name>
     			<param-value>classpath:springmvc-config.xml</param-value>
     		</init-param>
     		<load-on-startup>1</load-on-startup>
     	</servlet>
     	<servlet-mapping>
     		<servlet-name>springmvc</servlet-name>
     		<url-pattern>/</url-pattern>
     	</servlet-mapping>
     
     	<welcome-file-list>
     		<welcome-file>index.html</welcome-file>
     	</welcome-file-list>
     </web-app>
     ```

   > 到这里ssm的配置环境就搭建完毕，其中的扫描包路径请根据实际项目中的路径修改，拦截器、转换器、静态资源解放、驼峰命名法、懒加载等等，请根据需求自行设置

3. src下的各个实体类和接口类的编写，除mapper按照之前的方式即可

   - cn.wisdsoft.swg.pojo.Product.java

     ```java
     package cn.wisdsoft.swg.pojo;
     
     public class Product {
     	//属性
     	private Integer id;
     	private String p_name;
     	private Double price;
     	
     	//构造方法
     	public Product() {}
     
     	//getter and setter方法
     	public Integer getId() {
     		return id;
     	}
     	public void setId(Integer id) {
     		this.id = id;
     	}
     	public String getP_name() {
     		return p_name;
     	}
     	public void setP_name(String p_name) {
     		this.p_name = p_name;
     	}
     	public Double getPrice() {
     		return price;
     	}
     	public void setPrice(Double price) {
     		this.price = price;
     	}
     }
     ```

   - cn.wisdsoft.swg.mapper.ProductMapper.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
         "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
     <mapper namespace="cn.wisdsoft.swg.mapper.ProductMapper" >
     	<insert id="addProduct" parameterType="product" 
     		keyProperty="id" useGeneratedKeys="true">
     		insert into product (p_name, price) values (#{p_name},#{price})
     	</insert>
     	
     	<select id="findProduct" parameterType="product" resultType="product">
     		select * from product where 1 = 1 
     		<if test="p_name != null and p_name != ''">
     			and p_name like concat('%',#{p_name},'%')
     		</if>
     		<if test="price != null and price != 0">
     			and price = #{price}
     		</if>
     	</select>
     </mapper>
     ```

   - cn.wisdsoft.swg.mapper.ProductMapper.java

     ```java
     package cn.wisdsoft.swg.mapper;
     
     import java.util.List;
     
     import cn.wisdsoft.swg.pojo.Product;
     
     public interface ProductMapper {
     
     	public int addProduct(Product product);
     	
     	public List<Product> findProduct(Product product);
     }
     ```

     > 从此以后不需要再写Dao接口和它的实现类了，使用mapper.xml和mapper接口必须在同一包下，名字一致，方法名一致，参数一致，返回值一致。

   - cn.wisdsoft.swg.service.ProductService.java

     ```java
     package cn.wisdsoft.swg.service;
     
     import java.util.List;
     
     import cn.wisdsoft.swg.pojo.Product;
     
     public interface ProductService {
     	/**
     	 * 添加方法
     	 * @param product 产品对象
     	 * @return >0表示成功
     	 */
     	public int addProduct(Product product);
     	
     	/**
     	 * 根据条件查询列表
     	 * @param product 对象
     	 * @return 集合对象
     	 */
     	public List<Product> findProduct(Product product);
     }
     ```

   - cn.wisdsoft.swg.service.impl.ProductServiceImpl.java

     ```java
     package cn.wisdsoft.swg.service.impl;
     
     import java.util.List;
     
     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Service;
     import org.springframework.transaction.annotation.Transactional;
     
     import cn.wisdsoft.swg.mapper.ProductMapper;
     import cn.wisdsoft.swg.pojo.Product;
     import cn.wisdsoft.swg.service.ProductService;
     
     @Service
     @Transactional
     public class ProductServiceImpl implements ProductService {
     
     	@Autowired
     	private ProductMapper productMapper;
     	
     	@Override
     	public int addProduct(Product product) {
     		int i = productMapper.addProduct(product);
     		return i;
     	}
     
     	@Override
     	public List<Product> findProduct(Product product) {
     		List<Product> list = productMapper.findProduct(product);
     		return list;
     	}
     
     }
     ```

   - cn.wisdsoft.swg.controller.LoginController.java

     ```java
     package cn.wisdsoft.swg.controller;
     
     import javax.servlet.http.HttpServletRequest;
     import javax.servlet.http.HttpSession;
     
     import org.springframework.stereotype.Controller;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PostMapping;
     
     @Controller
     public class LoginController {
     
     	@GetMapping("/login")
     	public String toLogin() {
     		return "login";
     	}
     	
     	@PostMapping("/login")
     	public String login(HttpSession session,HttpServletRequest request, String username, String userpass) {
     		if (username.equals("admin") && userpass.equals("123")) {
     			session.setAttribute("user_session", username);
     			return "index";
     		} else {
     			request.setAttribute("msg", "用户名或密码错误！请重新输入！");
     			return "login";
     		}
     	}
     }
     ```

   - cn.wisdsoft.swg.controller.ProductController.java

     ```java
     package cn.wisdsoft.swg.controller;
     
     import java.util.List;
     
     import javax.servlet.http.HttpServletRequest;
     
     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Controller;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PostMapping;
     import org.springframework.web.bind.annotation.RequestMapping;
     
     import cn.wisdsoft.swg.pojo.Product;
     import cn.wisdsoft.swg.service.ProductService;
     
     @Controller
     public class ProductController {
     	
     	@Autowired
     	private ProductService productService;
     
     	@GetMapping("/toAddProduct")
     	public String toAddProduct() {
     		return "saveProduct";
     	}
     	
     	@PostMapping("/addProduct")
     	public String addProduct(Product product, HttpServletRequest request) {
     		int i = productService.addProduct(product);
     		if (i > 0) {
     			return "index";
     		} else {
     			request.setAttribute("msg", "添加失败");
     			return "saveProduct";
     		}
     	}
     	
     	@RequestMapping("/findProduct")
     	public String findProduct(Product product, HttpServletRequest request) {
     		List<Product> list = productService.findProduct(product);
     		request.setAttribute("list", list);
     		return "showProduct";
     	}
     }
     ```

4. 添加测试页面

   - /WEB-INF/jsp/login.jsp

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
     <html>
     <head>
     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
     <title>Insert title here</title>
     </head>
     <body>
     	<form action="/swg/login" method="post">
     		用户名：<input type="text" name="username" /><br/>
     		密码：<input type="password" name="userpass" /><br/>
     		<input type="submit" value="登录" />${msg}
     	</form>
     </body>
     </html>
     ```

   - /WEB-INF/jsp/index.jsp

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
     <html>
     <head>
     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
     <title>Insert title here</title>
     </head>
     <body>
     	欢迎:${user_session}登录系统<br/>
     	<a href="/swg/toAddProduct">添加产品信息</a><br/>
     	<a href="/swg/findProduct">查看产品信息</a><br/>
     	<a href="/swg/html/index.html">查看订单信息</a><br/>
     </body>
     </html>
     ```

   - /WEB-INF/jsp/saveProduct.jsp

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
     <html>
     <head>
     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
     <title>Insert title here</title>
     </head>
     <body>
     	<form action="/swg/addProduct" method="post">
     		产品名称：<input type="text" name="p_name" /><br/>
     		价格：<input type="text" name="price" /><br/>
     		<input type="submit" value="添加"/>${msg}
     	</form>
     </body>
     </html>
     ```

   - /WEB-INF/jsp/showProduct.jsp

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
     <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
     <html>
     <head>
     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
     <title>Insert title here</title>
     </head>
     <body>
     	<form action="/swg/findProduct" method="post">
     		名称： <input type="text" name="p_name" />价格： <input type="text" name="price" /><input type="submit" value="搜索" /><br/>
     	</form>
     	
     	<p>序号&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;产品名称&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;价格</p>
     	<c:forEach items="${list }" var="product">
     		<p>${product.id }&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;${product.p_name }&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;${product.price }</p>
     	</c:forEach>
     </body>
     </html>
     ```

5. 数据库swgds.sql

   ```sql
   /*
   Navicat MySQL Data Transfer
   
   Source Server         : test
   Source Server Version : 50562
   Source Host           : localhost:3306
   Source Database       : swgds
   
   Target Server Type    : MYSQL
   Target Server Version : 50562
   File Encoding         : 65001
   
   Date: 2020-11-12 12:15:38
   */
   
   SET FOREIGN_KEY_CHECKS=0;
   
   -- ----------------------------
   -- Table structure for orders
   -- ----------------------------
   DROP TABLE IF EXISTS `orders`;
   CREATE TABLE `orders` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `p_id` int(11) DEFAULT NULL,
     `total` decimal(18,4) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   
   -- ----------------------------
   -- Table structure for product
   -- ----------------------------
   DROP TABLE IF EXISTS `product`;
   CREATE TABLE `product` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `p_name` varchar(20) DEFAULT NULL,
     `price` decimal(18,4) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;
   ```

   > 如此一个简单的ssm操作系统就完成了，但他有很大的缺点，它只有jsp页面显示动态数据，我们可以根据前端的ajax创建api接口给html页面提供数据

6. 在src源目录下

   - cn.wisdsoft.swg.api.ProductController.java

     ```java
     package cn.wisdsoft.swg.api;
     
     import java.util.List;
     
     
     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Controller;
     import org.springframework.web.bind.annotation.RequestMapping;
     import org.springframework.web.bind.annotation.ResponseBody;
     
     import cn.wisdsoft.swg.pojo.Product;
     import cn.wisdsoft.swg.service.ProductService;
     
     @Controller
     public class ProductController {
     	
     	@Autowired
     	private ProductService productService;
     
     	@RequestMapping("/productlist")
     	@ResponseBody
     	public List<Product> findProduct(Product product) {
     		List<Product> list = productService.findProduct(product);
     		return list;
     	}
     }
     ```

   - 在WebContent下 /html/index.html

     ```html
     <!DOCTYPE html>
     <html>
     <head>
     <meta charset="UTF-8">
     <title>Insert title here</title>
     <script type="text/javascript" src="/swg/js/jquery-3.3.1.js"></script>
     </head>
     <body>
     	<div id="div"></div>
     </body>
     <script type="text/javascript">
     	$(function(){
     		$.ajax({
     			url : "/swg/productlist",
     			type : "get",
     			//data : "",
     			datatype : "json",
     			success:function(result){
     				console.log(result);
     				for(var i = 0; i < result.length; i++) {
     					$("#div").append("序号："+result[i].id+"，名称："+result[i].p_name+"，价格："+result[i].price+"<br/>");
     				}
     				
     			}
     		});
     	})
     </script>
     </html>
     ```

   - 在WebContent下 /js/jquery-3.3.1.js

     emmm，jquery.js代码太长，大家可以从网上自行搜索下载这里简单提供一个网址

     [BootCDN](https://www.bootcdn.cn/)

## 学习总结

- 跳转HTML页面

  1. 返回值为String类型，返回值前缀为"redirect:"和"forward:"

     相对于"redirect:"重定向，"forward:"转发更符合地址的隐秘性

     > 同理可得，在img等有src属性的请求路径标签，我们也可以拦截转发静态资源，隐藏真实路径

  2. <a>标签跳转

  3. js控制跳转

- ajax跨域问题

  在上面springmvc的工作流程时，我们提到过浏览器的地址路径由协议(http/https)、主机地址(ipv4/ipv6/域名)、端口号(默认是80)、uri(项目+项目路径)组成

  当协议、主机地址、端口号、项目任意一个不相同时，都会遇到跨域问题

  跨域问题的出现是为了防止垃圾信息的请求，恶意用户的访问等等才出现的

  解决跨域问题有两种：

  1. 前端请求数据类型用jsonp（没成功过，就不误人子弟了）

  2. 后台服务器项目开放权限

     在我们SSM框架有一个注解`@CrossOrigin`，将它添加到需要开放权限的方法上，就能通过ajax请求到

- API接口

  在前后端分离开放的模式下，我们可以将后台控制器Ctroller和前台ajax接口API分离开来，创建一个新的api包来专门存放API接口

