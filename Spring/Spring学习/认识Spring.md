> **Spring的特点：**
>
> 轻量级框架不需要过多的继承和实现接口
>
> 提供IOC容器：控制翻转
>
> AOP:面向切面的编程
>
> 对事物的支持
>
> 对框架的支持

主要的内容：

![img](E:\MarkdownPicture\2018052413503945.jpg)

**IOC**控制反转

```java 
package cn.leiyc.dao;
//持久层的接口
public interface UserDao {
	public void getUser();

}

```

```java
package cn.leiyc.dao.impl;
//持久层的实现
import cn.leiyc.dao.UserDao;

public class UserImpl implements UserDao{

	@Override
	public void getUser() {
	System.out.print("输出用户信息");
	}

}

```

```java 
package cn.leiyc.service;
//service服务层接口
public interface  UserService {
	public void getUser();

}
```

```java 
package cn.leiyc.service.impl;
//service服务层实现

import cn.leiyc.dao.UserDao;
import cn.leiyc.dao.impl.UserImpl;
import cn.leiyc.service.UserService;

public class UserServiceImpl implements UserService{
	private UserDao userdao = new UserImpl();//创建持久层的对象
	@Override
	public void getUser() {
		userdao.getUser();
		
	}

}

```

####按照上述的实现方式一旦修改持久层，那么也会波及到服务层的代码实现





```java 
package cn.leiyc.dao;

public interface UserDao {//持久层接口
	public void getUser();

}
```

```java
package cn.leiyc.dao.impl;

import cn.leiyc.dao.UserDao;

public class UserMysqlImpl implements UserDao{//mysql实现

	@Override
	public void getUser() {
	System.out.print("mysql实现");
	}

}

```

```java 
package cn.leiyc.dao.impl;

import cn.leiyc.dao.UserDao;

public class UserOracleImpl implements UserDao {//oracle实现

	@Override
	public void getUser() {
		System.out.println("oracle实现");
		
	}

}

```

```Java
package cn.leiyc.service;

import cn.leiyc.dao.UserDao;

public interface  UserService {
	 public void getUser();//获取实现 方法
	 public void setUser(UserDao userDao);//设置驻留在service层的dao对象

}


```

```java 
package cn.leiyc.service.impl;

import cn.leiyc.dao.UserDao;

import cn.leiyc.service.UserService;

public class UserServiceImpl implements UserService{
	private UserDao userdao = null ;//置空属性
	@Override
	public void setUser( UserDao userDao) {

		this.userdao = userDao;//给dao 具体的业务对象
		
	}
	@Override
	public void getUser() {
		userdao.getUser();
		/*
		 * 利用外部的属性传递调用对应的方法，
		 * 给了具体的dao实现，就调用该dao的具体实现方法
		 */
	}

}


```

```java
package cn.leiyc.test;

import org.junit.jupiter.api.Test;
import cn.leiyc.dao.impl.UserMysqlImpl;
import cn.leiyc.dao.impl.UserOracleImpl;
import cn.leiyc.service.impl.UserServiceImpl;

public class Usertest {
	@Test
	public void test1(){
		
		 UserServiceImpl serviceImpl1 = new UserServiceImpl();
		 serviceImpl1.setUser(new UserMysqlImpl());
		 serviceImpl1.getUser();
		 System.out.println();
		 System.out.println("---------------");
		 UserServiceImpl serviceImpl2 = new UserServiceImpl();
		 serviceImpl2.setUser(new UserOracleImpl());
		 serviceImpl2.getUser();
		
		
	}

}

```

####以上方式为了降低service与dao的耦合强度，将具体的dao实现类包装好成为传递参数。这样相对于第一种方式让具体的dao在service中可以将自己的任务信息派发出来而不需要依赖原来驻扎在service中的dao对象。应用程序不需要自己创建对象，可以利用读取配置文件来读取配置信息，利用反射创建对象。解耦合dao与service的关系，现在只关注业务的实现



IOC的特点：

对象由原来程序本身创建，转变程序只需要接受对象：用户配置配置文件；实现类的修改，改变传递的对象。

程序员主要关注业务的实现。

实现了dao层与service层的解耦合，没有直接的依赖的关系。

dao层的改变不会改变导致应用程序的改变，只需要修改相关的配置就可以。



hello Spring:

需要导入Spring的jar包

https://repo.spring.io/webapp/#/home

访问该网站下载

spring 需要创建配置使用的xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd">
 <!--bean is the object of java，bean就是Java对象-->
 <!--在这里Spring创建管理Java对象 -->
 <bean id="..." class="..." name="...">  
   
     <!-- collaborators and configuration for this bean go here -->
 
 </bean>
 <!-- more bean definitions go here -->
</beans>

```

利用Spring实现简单的hello输出：

```java 
package cn.leiyc.bean;

public class Hello {
	private String name ;
	private String sex;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getSex() {
		return sex;
	}

	public void setSex(String sex) {
		this.sex = sex;
	}
	
	public void show () {
		System.out.println("hello, "+name );
		System.out.println("sex, " + sex);
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
 <bean name="hello" class="cn.leiyc.bean.Hello">
 	<property name="name" value="zhangshang"></property>
 	<property name="sex" value="男"></property>
 </bean>
 <!-- more bean definitions go here -->
</beans>
```

```java
package cn.leiyc.test;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.leiyc.bean.Hello;

public class Hellotest {
	@Test
	public void test1() {
		//解析xm文件，生成管理的相应的bean 对象
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //通过Object org.springframework.beans.factory.BeanFactory.getBean(String arg0) throws BeansException获得名为hello的bean的类的名称，Spring利用反射机制得到该类
		Hello hello = (Hello) context.getBean("hello");
        //获得对象后调用对应的方法
		hello.show();
	}

}

```

针对这样的hello类似的bean 文件可以在配置文件中直接对属性和值做出相应的赋值操作。

####控制反转：

控制：指的是控制对象的创建，就是控制对象创建的权限；传统的应用的程序对象是有程序本身创建的，现在使用Spring容器来创建。

反转：程序本身不在主动的 创建对象，而是被动的接受对象的传递。对象创建主动权不在程序的身上，而是将对象创建的交给了使用者去创建。

###依赖注入：dependency injection

依赖的对象现在交给了容器去配置管理，容器设置对象的过程我们理解为依赖注入