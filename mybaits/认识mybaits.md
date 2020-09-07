> 什么是mybaits ?
>
> > MyBatis 是一个可以自定义SQL、存储过程和高级映射的持久层框架。MyBatis 摒除了大部分的JDBC代码、手工设置参数和结果集重获。MyBatis 只使用简单的XML 和注解来配置和映射基本数据类型、Map 接口和POJO 到数据库记录。相对Hibernate和Apache OJB等“一站式”ORM解决方案而言，Mybatis 是一种“半自动化”的ORM实现。
>
> 持久化：
>
> > 数据从瞬间状态变成持久化状态
>
> 持久持久层
>
> > 完成持久化工作的代码模块--dao层
>
> mybatis就是帮助程序员从数据库存取数据
>
> mybatis是有一种半自动化的ORM框架。O--Object，R--relationship，M--mapping
>
> **功能架构设计**：
>
> ![mybatis架构.PNG](http://s1.51cto.com/images/20171220/1513771840746590.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
>
> **功能架构讲解：**
>
> 我们把Mybatis的功能架构分为三层：
>
> (1) API接口层：提供给外部使用的接口API，开发人员通过这些本地API来操纵数据库。接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。
>
> MyBatis和数据库的交互有两种方式：
>
> a.使用传统的MyBatis提供的API；
>
> b. 使用Mapper接口
>
> a1.使用传统的MyBatis提供的API
>
> 这是传统的传递Statement Id 和查询参数给 SqlSession 对象，使用 SqlSession对象完成和数据库的交互；MyBatis 提供了非常方便和简单的API，供用户实现对数据库的增删改查数据操作，以及对数据库连接信息和MyBatis 自身配置信息的维护操作。 ![图片1.png](http://s1.51cto.com/images/20171220/1513771849318656.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
>
>    上述使用MyBatis 的方法，是创建一个和数据库打交道的SqlSession对象，然后根据Statement Id 和参数来操作数据库，这种方式固然很简单和实用，但是它不符合面向对象语言的概念和面向接口编程的编程习惯。由于面向接口的编程是面向对象的大趋势，MyBatis 为了适应这一趋势，增加了第二种使用MyBatis 支持接口（Interface）调用方式。
>
> 1.2. 使用Mapper接口
>
>  MyBatis 将配置文件中的每一个<mapper> 节点抽象为一个 Mapper 接口，而这个接口中声明的方法和跟<mapper> 节点中的<select|update|delete|insert> 节点项对应，即<select|update|delete|insert> 节点的id值为Mapper 接口中的方法名称，parameterType 值表示Mapper 对应方法的入参类型，而resultMap 值则对应了Mapper 接口表示的返回值类型或者返回结果集的元素类型。
>
> ![图片2.png](http://s1.51cto.com/images/20171220/1513771860796246.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
>
>  根据MyBatis 的配置规范配置好后，通过SqlSession.getMapper(XXXMapper.class) 方法，MyBatis 会根据相应的接口声明的方法信息，通过动态代理机制生成一个Mapper 实例，我们使用Mapper 接口的某一个方法时，MyBatis 会根据这个方法的方法名和参数类型，确定Statement Id，底层还是通过SqlSession.select("statementId",parameterObject);或者SqlSession.update("statementId",parameterObject); 等等来实现对数据库的操作
>
> MyBatis 引用Mapper 接口这种调用方式，纯粹是为了满足面向接口编程的需要。（其实还有一个原因是在于，面向接口的编程，使得用户在接口上可以使用注解来配置SQL语句，这样就可以脱离XML配置文件，实现“0配置”）。
>
>  
>
> (2) 数据处理层：负责具体的SQL查找、SQL解析、SQL执行和执行结果映射处理等。它主要的目的是根据调用的请求完成一次数据库操作。
>
>  (2.1).参数映射和动态SQL语句生成
>
> ​    动态语句生成可以说是MyBatis框架非常优雅的一个设计，MyBatis 通过传入的参数值，使用 Ognl 来动态地构造SQL语句，使得MyBatis有很强的灵活性和扩展性。
>
> 参数映射指的是对于java 数据类型和jdbc数据类型之间的转换：这里有包括两个过程：查询阶段，我们要将java类型的数据，转换成jdbc类型的数据，通过 preparedStatement.setXXX() 来设值；另一个就是对resultset查询结果集的jdbcType 数据转换成java 数据类型。
>
> ​    (2.2). SQL语句的执行以及封装查询结果集成List<E>
>
> ​       动态SQL语句生成之后，MyBatis 将执行SQL语句，并将可能返回的结果集转换成List<E> 列表。MyBatis 在对结果集的处理中，支持结果集关系一对多和多对一的转换，并且有两种支持方式，一种为嵌套查询语句的查询，还有一种是嵌套结果集的查询。
>
>  
>
> (3)基础支撑层：负责最基础的功能支撑，包括连接管理、事务管理、配置加载和缓存处理，这些都是共用的东西，将他们抽取出来作为最基础的组件。为上层的数据处理层提供最基础的支撑。
>
> **二、mybatis执行流程**
>
>  
>
> \1. 加载配置文件并初始化(SqlSession)
>
> 配置文件来源于两个地方，一个是配置文件(主配置文件conf.xml,mapper文件*.xml)，一个是java代码中的注释，将sql的配置信息加载成为一个mappedstatement对象，存储在内存之中（包括传入参数的映射配置，结果映射配置，执行的sql语句）。
>
> \2. 接收调用请求
>
> 调用mybatis提供的api，传入的参数为sql的id（有namespase和具体sql的id组成）和sql语句的参数对象，mybatis将调用请求交给请求处理层。
>
> \3. 处理请求
>
> 根据sql的id找到对应的mappedstatament对象。
>
> 根据传入参数解析mappedstatement对象，得到最终要执行的sql。
>
> 获取数据库连接，执行sql，得到执行结果
>
> Mappedstatement对象中的结果映射对执行结果进行转换处理，并得到最终的处理结果。
>
> 释放连接资源
>
> \4. 返回处理结果

> 如何使用Mybatis:
>
> > 导入Mybatis相关jar包；导入相关的数据库驱动包
> >
> > 编写相关的xml文件：mybatis-config.xml
> >
> > ```xml 
> > <?xml version="1.0" encoding="UTF-8" ?>
> > <!DOCTYPE configuration
> >  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
> >  "http://mybatis.org/dtd/mybatis-3-config.dtd">
> > <configuration>
> > 	<environments default="development">
> > 	 <environment id="development">
> > 	 	<transactionManager type="JDBC"/>
> > 		 <dataSource type="POOLED">
> > 		 	<property name="driver" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
> > 			<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
> > 			<property name="username" value="root"/><!-- 设置用户的值 -->
> > 		 	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
> > 		 </dataSource>
> > 	 </environment>
> > 	</environments>
> > 
> > 	<mappers>
> > 	 	<mapper resource="cn/sxt/entity/user.mapper.xml"/><!-- 导入mapper文件 -->
> > 	</mappers>
> > 
> > </configuration>
> > ```
> >
> > 
> >
> > 创建SqlSessionFactory获得SqlSession
> >
> > ```java 
> > package cn.sxt.utils;
> > 
> > import java.io.IOException;
> > import java.io.InputStream;
> > 
> > import org.apache.ibatis.io.Resources;
> > import org.apache.ibatis.session.SqlSession;
> > import org.apache.ibatis.session.SqlSessionFactory;
> > import org.apache.ibatis.session.SqlSessionFactoryBuilder;
> > 
> > public class MybatisUtil {
> > 	//创建SqlSessionFactory
> > 	public static SqlSessionFactory getSqlSessionFactory() throws IOException {
> > 		String resource = "mybatis-config.xml";
> > 		InputStream inputStream = Resources.getResourceAsStream(resource);
> > 		SqlSessionFactory sqlSessionFactory =
> > 		 new SqlSessionFactoryBuilder().build(inputStream);
> > 		return sqlSessionFactory;
> > 	}
> > 	//创建SqlSession
> > 	public static SqlSession getSqlSession() throws IOException {
> > 		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
> > 		return sqlSessionFactory.openSession();
> > 		
> > 	}
> > }
> > 
> > ```
> >
> > 创建实体类：（需要从数据库获取数据的类）
> >
> > ```java 
> > package cn.sxt.entity;
> > //实体类的定义，实体bean
> > public class User {
> > 	private int id;
> > 	private String name;
> > 	private String password;
> > 	public int getId() {
> > 		return id;
> > 	}
> > 	public void setId(int id) {
> > 		this.id = id;
> > 	}
> > 	public String getName() {
> > 		return name;
> > 	}
> > 	public void setName(String name) {
> > 		this.name = name;
> > 	}
> > 
> > 	public String getPwd() {
> > 		return password;
> > 	}
> > 	public void setPwd(String pwd) {
> > 		this.password = pwd;
> > 	}
> > 	@Override
> > 	public String toString() {
> > 		return "User [id=" + id + ", name=" + name + ", pwd=" + password + "]";
> > 	}
> > 
> > }
> > 
> > ```
> >
> > 编写sql语句的映射文件mapper.xml
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8" ?>
> > <!DOCTYPE mapper
> >  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
> >  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
> > <mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper的名字 -->
> >  	<select id="selectUser" resultType="cn.sxt.entity.User"><!--id 作为select辨识符号 resultType="包名和类名" -->
> >  		select * from user where id = #{id}<!-- sql语句，#{注入的值} -->
> >  	</select>
> > </mapper>
> > ```
> >
> > 编写测试类
> >
> > ```java
> > package cn.sxt.test;
> > 
> > import java.io.IOException;
> > 
> > import org.apache.ibatis.session.SqlSession;
> > 
> > import cn.sxt.entity.User;
> > import cn.sxt.utils.MybatisUtil;
> > 
> > public class Test {
> > 
> > 	public static void main(String[] args) throws IOException {
> > 		SqlSession sqlSession = MybatisUtil.getSqlSession();
> > 		User user = sqlSession.selectOne("cn.sxt.entity.UserMapper.selectUser", 6);
> > 		System.out.println(user.toString());
> > 	}
> > 
> > }
> > 
> > ```
> >
> > 

