删除掉mybatis-config.xml:指直接在Spring配置文件配置注入mapper文件

```java
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

import org.mybatis.spring.support.SqlSessionDaoSupport;

import cn.sxt.dao.UserDao;
import cn.sxt.entity.User;

public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao{
	@Override
	public List<User> selectUser() {
		return getSqlSession().selectList("cn.sxt.entity.UserMapper.selectUser");
	}
	
}

```

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

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper的名字 -->
 	<select id="selectUser" resultType="cn.sxt.entity.User"><!--id 作为select辨识符号 resultType="包名和类名" -->
 		select * from user 
 	</select>
 	<insert id="add" parameterType="cn.sxt.entity.User" useGeneratedKeys="true">
 		insert into user (name,password) values (#{name},#{password});
 	</insert>
 	<delete id="remove">
 		delete from user where id=#{id};
 	</delete>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:tx="http://www.springframework.org/schema/tx"  
    xmlns:aop="http://www.springframework.org/schema/aop"  
    xsi:schemaLocation="
 http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/tx
 https://www.springframework.org/schema/tx/spring-tx.xsd
 http://www.springframework.org/schema/aop
 https://www.springframework.org/schema/aop/spring-aop.xsd">
 		<!-- 配置数据源 -->
 <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
 	<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
	<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
	<property name="username" value="root"/><!-- 设置用户的值 -->
	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
 </bean>
 <!-- 声明式事务配置 -->
<!-- 配置事务管理器 -->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>
<!-- 配置事务通知-->
<tx:advice id="txAdvice" transaction-manager="txManager">
	<tx:attributes>
	<!-- 配置哪些方法使用什么样的事务，和事务的传播方法 -->
		<tx:method name="add" propagation="REQUIRED"/>
		<tx:method name="*" propagation="REQUIRED"/>
		<tx:method name="get" read-only="true"/>
	</tx:attributes>
</tx:advice>
<!-- 配置aop -->
<aop:config>
	<aop:pointcut expression="execution(* cn.sxt.dao.impl.*.*(..))" id="pointcut"/>
	<aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut"/>

</aop:config>
 <!-- 配置sqlSessionFactory -->
 <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
 	<property name="dataSource" ref="dataSource"></property>
 	<property name="mapperLocations" value="cn/sxt/entity/*.mapper.xml"></property>
 </bean>
  <bean id="userDao" class="cn.sxt.dao.impl.UserDaoImpl">
  	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
  </bean>
</beans>
```

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

