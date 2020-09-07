

### 第二十三章，Spring中的事务

#### 1.什么是事务 ？

~~~markdown
* 保证业务操作的完整性的一种数据库机制

* 事务的4特点： A C I D
* 1.A 事务的原子性
* 2.C 事务的一致性
* 3.I 事务的隔离性
* 4.D 事务的持久性
~~~

#### 2.如何控制事务

~~~ markdown
JDBC :
	Connection.setAutoCommit(false);
	Connection.commit();
	COnnection.rollback();




Mybatis:
	Mybatis自动开启事务
	sqlSession.commit();
	sqlSession.rollback();
	
结论：控制事务的底层 都是通过Connection对象完成的。
~~~

#### 3.Spring控制事务的开发

~~~markdown
* Spring是通过AOP的方式进行事务的开发
	* 原始对象
		* 开发的时候的Service实现类，就是这些业务处理与DAO调用
		* DAO作为Service成员变量，依赖注入的方式赋值
	* 额外功能
		* MethodInterceptor
		* @Aspect
		* @Around
	* 切入点
		* @Transactional
		* 类上 ：类中所有的方法都会加入事务
		* 方法上：这个方法会加入事务
	* 组装切面
	 	* 切入点
	 	* 额外功能
	 	* <tx:annotation-driven transaction-mapper="">
~~~

#### 4. Spring控制事务的编码

~~~xml
搭建开发环境
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.6.RELEASE</version>
</dependency>
~~~

* 创建原始对象

  ~~~java
  package com.leiyc.impl;
  
  import com.leiyc.dao.UserDAO;
  import com.leiyc.entity.User;
  import com.leiyc.service.UserService;
  import org.springframework.transaction.annotation.Transactional;
  
  /**
   * @Author: Colin
   * @Date: 2020/8/17 23:25
   * @Version: 1.0
   * @Description:
   */
  @Transactional//重点
  public class UserServiceImpl implements UserService {
      private UserDAO userDAO;
  
      public UserDAO getUserDAO() {
          return userDAO;
      }
  
      public void setUserDAO(UserDAO userDAO) {
          this.userDAO = userDAO;
      }
  
      @Override
      public void register(User user) {
          userDAO.save(user);
      }
  }
  
  ~~~

  

* 相关配置

* ~~~xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/tx
          http://www.springframework.org/schema/tx/spring-tx.xsd
         ">
      <!--    配置连接池的bean作为待会的创建-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
          <property name="url" value="jdbc:mysql://localhost:3306/spring?serverTimezone=UTC"></property>
          <property name="username" value="root"></property>
          <property name="password" value="qqzz1776109898"></property>
      </bean>
      <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource"></property>
          <property name="typeAliasesPackage" value="com.leiyc.entity"></property>
          <property name="mapperLocations" >
              <list>
                  <value>classpath:mapper/*Mapper.xml</value>
              </list>
          </property>
      </bean>
      <!--        创建DAO 对象，MapperScannerConfigure-->
      <bean id="scanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryBean"></property>
          <property name="basePackage" value="com.leiyc.dao"></property>
      </bean>
      <bean id="userService" class="com.leiyc.impl.UserServiceImpl">
          <property name="userDAO" ref="userDAO"></property>
      </bean>
      <!--    DataSourceTransactionManager         -->
      <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSOurce" ref="dataSource"></property>
      </bean>
  
      <tx:annotation-driven transaction-manager="dataSourceTransactionManager"></tx:annotation-driven>
  
  </beans>
  ~~~



