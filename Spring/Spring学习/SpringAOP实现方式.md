

> 需要的外部依赖包：
>
> > aopalliance-1.0.jar
>
> > aspectjweaver-1.9.5.jar

**SpringAOP主要围绕以下概念展开：**

切面（Aspect）一个关注点的模块化，这个关注点可能会横切多个对象
连接点（Joinpoint）程序执行过程中某个特定的连接点
通知（Advice） 在切面的某个特的连接点上执行的动作
切入点（Pointcut）匹配连接的断言，在Aop中通知和一个切入点表达式关联
引入（Intruduction） 在不修改类代码的前提下，为类添加新的方法和属性
目标对象（Target Object） 被一个或者多个切面所通知的对象
Aop代理（AOP Proxy） AOP框架创建的对象，用来实现切面契约（aspect contract）（包括方法执行等）
织入（Weaving）把切面连接到其他的应用程序类型或者对象上，并创建一个被通知的对象，氛围：编译时织入，类加载时织入，执行时织入

**通知类型Advice:**

前置通知（before advice） 在某个连接点（jion point）之前执行的通知，但不能阻止连接点前的执行（除非抛出一个异常）
返回后通知（after returning advice）在某个连接点（jion point）正常执行完后执行通知
抛出异常通知（after throwing advice） 在方法异常退出时执行的通知
后通知（after(finally) advice）在方法抛出异常退出时候的执行通知（不管正常返回还是异常退出）
环绕通知（around advice) 包围一个连接点（jion point)的通知

 

**切入点Pointcut:SpringAOP占时仅仅支持方法的连接点**

例如定义切入点表达式 execution(* com.sample.service.impl..*.*(..))
execution()是最常用的切点函数，其语法如下所示：
 整个表达式可以分为五个部分：
 1、execution(): 表达式主体。
 2、第一个*号：表示返回类型，*号表示所有的类型。
 3、包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包，com.sample.service.impl包、子孙包下所有类的方法。
 4、第二个*号：表示类名，*号表示所有的类。
 5、*(..):最后这个星号表示方法名，*号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数。

execution(public * *(..)) 切入点为执行所有的public方式时
execution(* set*(..)) 切入点执行所有的set开始的方法时
execution(* com.xyz.service.Account.*(..)) 切入点执行Account类的所有方法时
execution(* com.xyz.service.*.*(..))切入点执行com.xyz.service包下的所有方法时
execution(* com.xyz.service..*.*(..)) 切入点执行com.xyz.service包以及其子包的所有的方法时

我们在Java项目开发中，AOP常见的配置有如下3种：

> AOP 实现方式： 
>
> > 第一种：通过Spring 的API实现
> >
> > 
> >
> > 第二种：自定义方法来实现
> >
> > ```java 
> > package cn.sxt.aop;
> > import java.lang.reflect.Method;
> > public class Log  {
> > 	public void before() {
> > 		System.out.println("----方法执行前----");
> > 	}
> > 	public void after() {
> > 		System.out.println("----方法执行后----");
> > 	}
> > }
> > 
> > ```
> >
> > ```java
> > package cn.sxt.service;
> > 
> > public interface UserService {
> > 	public 	void add();
> > 	public 	void update();
> > 	public 	void delete();
> > 	public 	void search();
> > }
> > 
> > ```
> >
> > ```java 
> > package cn.sxt.service;
> > 
> > public class UserServiceImpl implements UserService {
> > 
> > 	public void add() {
> > 		System.out.println("增加用户");
> > 	}
> > 
> > 	public void update() {
> > 		System.out.println("修改用户");
> > 	}
> > 
> > 	public void delete() {
> > 		System.out.println("删除用户");
> > 	}
> > 
> > 	public void search() {
> > 		System.out.println("查询用户");
> > 	}
> > 
> > 	
> > 
> > }
> > 
> > ```
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8"?>  
> > <beans xmlns="http://www.springframework.org/schema/beans"  
> >     xmlns:context="http://www.springframework.org/schema/context"  
> >     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx"  
> >     xmlns:aop="http://www.springframework.org/schema/aop"  
> >     xsi:schemaLocation="http://www.springframework.org/schema/beans  
> >       http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
> >  http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.2.xsd">
> >  <!--需要切入的方法类-->
> >  <bean id="userService" class="cn.sxt.service.UserServiceImpl"></bean>
> >   <!--提供切入的方法类-->
> >  <bean id="log" class="cn.sxt.aop.Log"></bean>
> >  <aop:config>
> >  	<aop:aspect ref="log">
> >  		<aop:pointcut expression="execution(* cn.sxt.service.UserService.*(..))" id="pointcut"/>
> >  		<aop:before method="before" pointcut-ref="pointcut"/>
> >  		<aop:after method="after" pointcut-ref="pointcut" />
> >  	</aop:aspect>
> >  </aop:config>
> > </beans>
> > ```
> >
> > 第三种：注解实现
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8"?>  
> > <beans xmlns="http://www.springframework.org/schema/beans"  
> >     xmlns:context="http://www.springframework.org/schema/context"  
> >     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx"  
> >     xmlns:aop="http://www.springframework.org/schema/aop"  
> >     xsi:schemaLocation="http://www.springframework.org/schema/beans  
> >       http://www.springframework.org/schema/beans/spring-beans-3.2.xsd  
> >  http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.2.xsd">
> >  <!--需要切入的方法类-->
> >  <bean id="userService" class="cn.sxt.service.UserServiceImpl"></bean>
> >   <!--提供切入的方法类-->
> >  <bean id="log" class="cn.sxt.aop.Log"></bean>
> > <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
> > </beans>
> > ```
> >
> > ```java
> > package cn.sxt.test;
> > 
> > import org.springframework.context.ApplicationContext;
> > import org.springframework.context.support.ClassPathXmlApplicationContext;
> > 
> > import cn.sxt.service.UserService;
> > 
> > public class Test {
> > 	public static void main(String[] args) {
> > 		ApplicationContext ac = new ClassPathXmlApplicationContext("beans.xml");
> > 		UserService userService = (UserService)ac.getBean("userService");
> > 		userService.add();
> > 	}
> > }
> > 
> > ```
> >
> > ```java 
> > package cn.sxt.aop;
> > 
> > import org.aspectj.lang.ProceedingJoinPoint;
> > import org.aspectj.lang.annotation.After;
> > import org.aspectj.lang.annotation.Around;
> > import org.aspectj.lang.annotation.Aspect;
> > import org.aspectj.lang.annotation.Before;
> > @Aspect
> > public class Log  {
> > 	@Before("execution(* cn.sxt.service.UserService.*(..))")
> > 	public void before() {
> > 		System.out.println("----方法执行前----");
> > 	}
> > 	@After("execution(* cn.sxt.service.UserService.*(..))")
> > 	public void after() {
> > 		System.out.println("----方法执行后----");
> > 	}
> > 	@Around("execution(* cn.sxt.service.UserService.*(..))")
> > 	public void around(ProceedingJoinPoint pj) {
> > 		System.out.println("环绕前执行");
> > 		System.out.println("签名"+pj.getSignature());
> > 		try {
> > 			//执行目标方法
> > 			pj.proceed();
> > 		} catch (Throwable e) {
> > 			// TODO Auto-generated catch block
> > 			e.printStackTrace();
> > 		}
> > 		System.out.println("环绕后执行");
> > 		
> > 	}
> > }
> > 	
> > ```
> >
> > ```java 
> > package cn.sxt.service;
> > 
> > public interface UserService {
> > 	public 	void add();
> > 	public 	void update();
> > 	public 	void delete();
> > 	public 	void search();
> > }
> > 
> > ```
> >
> > ```java 
> > package cn.sxt.service;
> > 
> > public class UserServiceImpl implements UserService {
> > 
> > 	public void add() {
> > 		System.out.println("增加用户");
> > 	}
> > 
> > 	public void update() {
> > 		System.out.println("修改用户");
> > 	}
> > 
> > 	public void delete() {
> > 		System.out.println("删除用户");
> > 	}
> > 
> > 	public void search() {
> > 		System.out.println("查询用户");
> > 	}
> > 
> > 	
> > 
> > }
> > 
> > ```
> >
> > 

AOP的重要性：使得业务 逻辑变得纯粹

参考：https://www.cnblogs.com/xuyatao/p/8485851.html