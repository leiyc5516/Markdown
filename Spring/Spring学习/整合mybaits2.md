修改了mybatis的bug ,利用SqlSessionDaoSupport和Spring中Bean注入

具体详见官方文档

案例：

```java 

public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao{
	@Override
	public List<User> selectUser() {
		return getSqlSession().selectList("cn.sxt.entity.UserMapper.selectUser");
	}
	
}
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
 	<property name="configLocation" value="classpath:mybatis-config.xml"></property>
 </bean>
  <bean id="userDao" class="cn.sxt.dao.impl.UserDaoImpl">
  	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
  </bean>
</beans>
```

差别对别mybatis整合1.