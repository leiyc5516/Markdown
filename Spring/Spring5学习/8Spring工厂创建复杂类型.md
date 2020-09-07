#### 简单对象：指的是可以直接通过new构造方法创建对象

#### 复杂对象：指的是不能够通过直接的new构造方法创建的对象

![image-20200809140707860](E:\Markdown\Spring\Spring5学习\image-20200809140707860.png)

### 1.Spring创建复杂对象的3中方式

#### 1.1FactoryBean工厂模式

##### 1.1.1实现FactoryBean接口

![image-20200809142604931](E:\Markdown\Spring\Spring5学习\image-20200809142604931.png)

~~~java
package org.example.FactoryBean;

import org.springframework.beans.factory.FactoryBean;

import java.sql.Connection;
import java.sql.DriverManager;

/**
 * @Author: Colin
 * @Date: 2020/8/9 14:29
 * @Version: 1.0
 * @Description:
 */
public class ConnectionFactoryBean implements FactoryBean<Connection> {
    //
    @Override
    public Connection getObject() throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/studydata?serverTimezone=UTC","root","qqzz1776109898" );
        return con;
    }

    @Override
    public Class<?> getObjectType() {
        return Connection.class;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}

~~~

##### 1.1.2Spring配置文件

~~~xml
#如果Class中指定的类型是FactoryBean接口的实现类，那么通过id值获得是这个类创建的复杂对象
<bean id="con" class="org.example.FactoryBean.ConnectionFactoryBean"></bean>
~~~

* 细节

  * 如果就想要获得FactoryBean类型的对象ctx.getBean("&con")这样就可以获得ConnectionFactoryBean
  * isSingleton()方法返回一个true只会创建一个，否者创建多个对象

* FactoryBean实现原理

* ~~~markdown
  接口回调
  	为什么Spring要规定一个FactoryBean接口让我们去创建复杂对象，实现getObject对象？
  	ctx.getBean("conn")为什么是获得复杂对象而不是简单的本源的那个类的对象 ctx.getBean("&con")才能获得本源类？
  ~~~

* ~~~markdown
  spring的内部运行的步骤
  	通过conn获得ConnectionFactoryBean类的对象，进而通过instanceof判断接口的实现类
  	Spring按照规定getObject()---->Connection
  	返回Connection
  ~~~

![image-20200809163818457](E:\Markdown\Spring\Spring5学习\image-20200809163818457.png)

* 总结FactoryBean

~~~markdown
Spring 中用于创建复杂对象的一种方式，也是Spring原生提供 后续需要整合Spring一系列其他框架需要使用FactoryBean的技术
~~~



#### 1.2实例工厂

~~~markdown
	1.避免Spring框架的侵入
	2.整合遗留的系统
~~~

~~~xml
 <bean id="connFactory" class="org.example.FactoryBean.ConnectionFactory"></bean>
<!-- 先创建对象，后调用方法-->
  <bean id="conn" factory-bean="connFactory" factory-method="getConnection"></bean>
~~~



#### 1.3静态工厂

~~~xml
<bean id="conn2" class="org.example.FactoryBean.StaticConnectionFacotry" factory-method="getConnection"></bean>
<!-- 不需要创建对象，直接调用方法-->
~~~

### 2.Spring创建对象的总结

![image-20200809170018022](E:\Markdown\Spring\Spring5学习\image-20200809170018022.png)