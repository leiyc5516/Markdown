> IOC实现原理：
>
> IOC的实现是通过IOC容器来实现的，IOC容器其实是一个BeanFactory
>
> 通过工厂模式 来创建类的，在Bean标签中含有scope属性，可以设置....  

利用Spring简单的实现dao与service分离配置：

```java 
package cn.leiyc.dao;

public interface UserDao {
	public void getUserDao();
	public void setUserDao();

}

```

```java
package cn.leiyc.dao.impl;

import cn.leiyc.dao.UserDao;

public class UserMysqlImpl implements UserDao{
	@Override
	public void getUserDao() {
		System.out.println("调用MySQL");
	}

	@Override
	public void setUserDao() {
		
		
	}

}

```

```java 
package cn.leiyc.dao.impl;

import cn.leiyc.dao.UserDao;

public class UserOracleImpl implements UserDao {

	@Override
	public void getUserDao() {
		System.out.println("调用Oracle");
		
	}

	@Override
	public void setUserDao() {
		
		
	}

}

```

```java  
package cn.leiyc.service;

import cn.leiyc.dao.UserDao;

public interface  UserService {
	 public void getUser();
	 public void setUserDao(UserDao userDao);

}

```

```java 
package cn.leiyc.service.impl;

import cn.leiyc.dao.UserDao;

import cn.leiyc.service.UserService;

public class UserServiceImpl implements UserService{
	private UserDao userao = null ;//置空属性
	@Override
	public void setUserDao( UserDao userDao) {

		this.userao = userDao;//给dao 具体的业务对象
		
	}
	@Override
	public void getUser() {
		userao.getUserDao();
		/*
		 * 利用外部的属性传递调用对应的方法，
		 * 给了具体的dao实现，就调用该dao的具体实现方法
		 */
	}

}

```

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd">
 <!--bean is the object of  java-->
	<bean id="Mysql" class="cn.leiyc.dao.impl.UserMysqlImpl"> 
	</bean>
	<bean id="Oracle" class="cn.leiyc.dao.impl.UserOracleImpl"> 
	</bean>
	<bean id="service" class="cn.leiyc.service.impl.UserServiceImpl">
		<property name="userDao" ref="Mysql"></property>
		<!-- name值为set方法后面的名称首字母小写，value表示普通的值，而针对对象这种复杂的值我们使用ref="" -->
	</bean>
 <!-- more bean definitions go here -->
</beans>
```

```java 
package cn.leiyc.test;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.service.UserService;

public class Usertest {
	@Test
	public void test1(){
		
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		UserService us =  (UserService)context.getBean("service");
		us.getUser();
		
		
	}

}

```

###IOC创建对象的三种方式

**1.无参函数的构造方法来创建，利用set方法实现属性的赋值**

```java
package cn.leiyc.vo;

public class User  {
	private String name;
	
	public User() {//无参的构造方式
		System.out.println("user的无参构造方法");
	}
	//set方式
	public void setName(String name) {
		this.name = name;
	}
	//
	public void show() {
		System.out.println("name = "+name);
	}

}

```

```java
package cn.leiyc.test;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.vo.User;

public class UserTest {
	@Test
	public void test1() {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		User user = (User) context.getBean("user");//根据xml配置文件ID与类的名字来创建对象
		user.show();
		
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd">
 <!--bean is the object of  java-->
	<bean id="user" class="cn.leiyc.vo.User">
	<property name="name" value="zhangsan"></property>
	</bean>
 <!-- more bean definitions go here -->
</beans>
```

**输出结果：**

```java 
/*
*user的无参构造方法
*name = zhangsan
*/
```

**2.通过有参的构造的方法来创建对象**

```java 
package cn.leiyc.vo;

public class User  {
	private String name;
	
	/*public void setName(String name) {
	*this.name = name;
	*}
	*/
	public User(String name) {
		super();
		this.name = name;
		System.out.println("user有参的构造方法");
	}
	/*public User() {无参的构造方式
	*	System.out.println("user的无参构造方法");
	*}
	*/
	public void show() {
		System.out.println("name = "+name);
	}

}

```

```java
package cn.leiyc.test;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.vo.User;

public class UserTest {
	@Test
	public void test1() {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		User user = (User) context.getBean("user1");//根据xml配置文件ID与类的名字来创建对象
		user.show();
		
	}

}

```

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd">
 <!--bean is the object of  java-->
	<!--
	<bean id="user" class="cn.leiyc.vo.User">
		<property name="name" value="zhangsan"></property>
	</bean>
	-->
	<bean id="user1" class="cn.leiyc.vo.User">
		<!-- index标记第几个参数，value标注值 -->
		<constructor-arg index="0" value="李四"></constructor-arg>
	</bean>
 <!-- more bean definitions go here -->
</beans>
```

**输出结果：**

```java
/*
*user有参的构造方法
*name = 李四
*/
```

**在有参构造的方式中有三种beans.xml配置方法：**

1）根据下标index来设置

```xml 
<bean id="user1" class="cn.leiyc.vo.User">
		<!-- index标记第几个参数，value标注值 -->
		<constructor-arg index="0" value="李四"></constructor-arg>
</bean>
```

2)根据参数名来配置

```xml
<bean id="user1" class="cn.leiyc.vo.User">
		<!--name是获取参数名，value来赋值  -->
		<constructor-arg name="gender" value ="man"></constructor-arg>
</bean>
```

3)根据参数类型 来设置

```xml
<bean id="user" class="cn.leiyc.vo.User">
		<!--  type值得类型，value是指值-->
		<constructor-arg type="java.lang.String" value="李四"></constructor-arg>
	</bean>
```

**3.通过工厂方法来创建对象**

1）静态工厂

```java
package cn.leiyc.vo;

public class User  {
	private String name;
	
	public void setName(String name) {
		this.name = name;
	}
	public User(String name) {
		super();
		this.name = name;
		System.out.println("user有参的构造方法");
	}
	public User() {//无参的构造方式
		System.out.println("user的无参构造方法");
	}
	public void show() {
		System.out.println("name = "+name);
	}

}

```



```java
package cn.leiyc.factory;

import cn.leiyc.vo.User;

public class UserFactory {//利用user中的有参构造方法，来工厂化创建
	public static  User newInstance(String name ) {
		return new User(name);	
	}

}
```

```java
package cn.leiyc.test;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.vo.User;

public class UserTest {
	@Test
	public void test1() {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		User user = (User) context.getBean("factory");//根据xml配置文件ID与类的名字来创建对象
		user.show();
		
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd">
 <!--bean is the object of  java-->
	<bean id="user" class="cn.leiyc.vo.User">
		<property name="name" value="zhangsan"></property>
	</bean>
	<bean id="user1" class="cn.leiyc.vo.User">
		<!-- index标记第几个参数，value标注值 -->
		<constructor-arg index="0" value="李四"></constructor-arg>
	</bean>
		<bean id="factory" class="cn.leiyc.factory.UserFactory" factory-method="newInstance">
		<!-- class配置的是factory ,factory-method指定工厂类创建对象的方法，下方指定该方法使用的参数，与对应的值-->
		<constructor-arg index="0" value="王五"></constructor-arg>
	</bean>
 <!-- more bean definitions go here -->
</beans>
```

**输出结果：**

```java
/*
*user的无参构造方法
*user有参的构造方法
*user有参的构造方法
*name = 王五
*/
```

2）动态工厂

相对于静态工厂，只是改变了beans.xml配置和factory的代码；

```xml
<bean id = "factory" class="cn.leiyc.factory.UserDynamicFactory" ></bean>
	<bean id="user2" factory-bean = "factory" factory-method="newInstance">
		<!-- class配置的是factory ,factory-method指定工厂类创建对象的方法，下方指定该方法使用的参数，与对应的值-->
		<constructor-arg index="0" value="赵六"></constructor-arg>
	</bean>
```

```java 
package cn.leiyc.factory;

import cn.leiyc.vo.User;

public class UserDynamicFactory {
	public  User newInstance(String name ) {
		return new User(name);	
	}
}

```

```java 
package cn.leiyc.test;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.vo.User;

public class UserTest {
	@Test
	public void test1() {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		User user = (User) context.getBean("user2");//根据xml配置文件ID与类的名字来创建对象
		user.show();
		
	}

}

```

```java
/*
*user的无参构造方法
*user有参的构造方法
*user有参的构造方法
*name = 赵六
*/
```

**动态工厂就是把自己的静态方法去掉static ,在xml配置文件中先注明动态工厂bean再通过一般的配置手段来实现配置**。例如

```xml 
<bean id = "factory" class="cn.leiyc.factory.UserDynamicFactory" ></bean>
	<bean id="user2" factory-bean = "factory" factory-method="newInstance">
		<!-- class配置的是factory ,factory-method指定工厂类创建对象的方法，下方指定该方法使用的参数，与对应的值-->
		<constructor-arg index="0" value="赵六"></constructor-arg>
	</bean>
```
