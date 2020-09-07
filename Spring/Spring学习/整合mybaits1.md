> 步骤
>
> > 导入相关jar包
>
> 对于Spring与mybatis整合的jar包：
>
> ant-1.10.3.jar
> ant-launcher-1.10.3.jar
> aopalliance-1.0.jar
> asm-7.0.jar
> aspectjweaver-1.9.5.jar
> cglib-3.2.10.jar
> commons-logging-1.2.jar
> javassist-3.24.1-GA.jar
> log4j-1.2.17.jar
> log4j-api-2.11.2.jar
> log4j-core-2.11.2.jar
> mybatis-3.5.3.jar
> mybatis-spring-2.0.3.jar
> mysql-connector-java-8.0.18.jar
> ognl-3.2.10.jar
> slf4j-api-1.7.26.jar
> slf4j-log4j12-1.7.26.jar
> spring-aop-5.2.3.RELEASE.jar
> spring-aspects-5.2.3.RELEASE.jar
> spring-beans-5.2.3.RELEASE.jar
> spring-context-5.2.3.RELEASE.jar
> spring-context-indexer-5.2.3.RELEASE.jar
> spring-context-support-5.2.3.RELEASE.jar
> spring-core-5.2.3.RELEASE.jar
> spring-expression-5.2.3.RELEASE.jar
> spring-instrument-5.2.3.RELEASE.jar
> spring-jcl-5.2.3.RELEASE.jar
> spring-jdbc-5.2.3.RELEASE.jar
> spring-jms-5.2.3.RELEASE.jar
> spring-messaging-5.2.3.RELEASE.jar
> spring-orm-5.2.3.RELEASE.jar
> spring-oxm-5.2.3.RELEASE.jar
> spring-test-5.2.3.RELEASE.jar
> spring-tx-5.2.3.RELEASE.jar
> spring-web-5.2.3.RELEASE.jar
> spring-webflux-5.2.3.RELEASE.jar
> spring-webmvc-5.2.3.RELEASE.jar
> spring-websocket-5.2.3.RELEASE.jar
>
> > 编写配置文件
> >
> > 实现

Dao层

```Java
package cn.sxt.dao;

import java.util.List;

import cn.sxt.entity.User;

public interface UserDao {
	public List<User> selectUser();
}

```

```java
package cn.sxt.dao.impl;

import java.util.List;

import org.mybatis.spring.SqlSessionTemplate;

import cn.sxt.dao.UserDao;
import cn.sxt.entity.User;

public class UserDaoImpl implements UserDao{
	private SqlSessionTemplate sqlSession;
	public void setSqlSession(SqlSessionTemplate sqlSession) {
		this.sqlSession = sqlSession;
	}
	@Override
	public List<User> selectUser() {
		return sqlSession.selectList("cn.sxt.entity.UserMapper.selectUser");
	}
	
}

```

实体类

```java
package cn.sxt.entity;
//实体类的定义，实体bean
public class User {
	private int id;
	private String name;
	private String password;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}

	public String getPwd() {
		return password;
	}
	public void setPwd(String pwd) {
		this.password = pwd;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", pwd=" + password + "]";
	}

}

```

配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper的名字 -->
 	<select id="selectUser" resultType="cn.sxt.entity.User"><!--id 作为select辨识符号 resultType="包名和类名" -->
 		select * from user 
 	</select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx"  
    xmlns:aop="http://www.springframework.org/schema/aop"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
      	http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
		http://www.springframework.org/schema/aop 
 		http://www.springframework.org/schema/aop/spring-aop-3.2.xsd">
 		<!-- 配置数据源 -->
 <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
 	<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
	<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
	<property name="username" value="root"/><!-- 设置用户的值 -->
	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
 </bean>
 <!-- 配置sqlSessionFactory -->
 <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
 	<property name="dataSource" ref="dataSource"></property>
 	<property name="configLocation" value="classpath:mybatis-config.xml"></property>
 </bean>
 
  <!-- 配置Dao层 -->
  <!-- sqlSessionTemplate与sqlSessionFactory建立关系 -->
  <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
  	<constructor-arg index="0" ref="sqlSessionFactory"></constructor-arg>
  </bean>
  <bean id="userDao" class="cn.sxt.dao.impl.UserDaoImpl">
  	<property name="sqlSession" ref="sqlSessionTemplate"></property>
  </bean>
</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!-- 根节点文件 -->
	<environments default="development">
	<!-- 
	environments指的是可以配置多个环境  default 指向默认的环境
	每个环境都会创建对应的 SqlSessionFactory
	-->
	 <environment id="development">
	 	<!-- MyBatis的事务管理分为两种形式：

		（1）使用JDBC的事务管理机制。
		
		这种机制就是利用java.sql.Connection对象完成对事务的提交
		
		（2）使用MANAGED的事务管理机制。
		
		这种机制mybatis自身不会去实现事务管理，而是让程序的Web容器或者Spring容器来实现对事务的管理。
		 -->
	 	<transactionManager type="JDBC"/>
	 	
	 	<!-- 
	 	数据库源类型：
	 	UNPOOLED：需要的时候打开或关闭
	 	POOLED：使用数据库连接池
	 	 -->
		 <dataSource type="POOLED">
		 	<property name="driver" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
			<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
			<property name="username" value="root"/><!-- 设置用户的值 -->
		 	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
		 </dataSource>
	 </environment>
	</environments>
	<!-- 定义了映射SQL语句的文件 -->
	<mappers>
	 	<mapper resource="cn/sxt/entity/user.mapper.xml"/><!-- 导入mapper文件 -->
	 	<mapper class="cn.sxt.dao.UserDao"/><!-- 导入mapper文件 -->
	</mappers>

</configuration>
```

测试文件

```java 
package cn.sxt.test;


import java.io.IOException;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.sxt.dao.UserDao;

public class Test {

	public static void main(String[] args) throws IOException {
		ApplicationContext ac = new ClassPathXmlApplicationContext("Beans.xml");
		UserDao userDao =(UserDao)ac.getBean("userDao");
		System.out.println(userDao.selectUser().size());
	}

}

```

