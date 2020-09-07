### 第十九章，基于注解的AOP 编程

#### 1.基于注解的AOP编程的开发步骤

~~~markdown
* 创建原始类
* 编写额外功能
* 找到切入点
* 组装切入点和额外功能
~~~



~~~java
/*
标注为切面类
 */
@Aspect
public class MyAspect {
    @Around("execution(* login(..))")/*标注切入点*/
    /*等同invoke方法*/
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("----log-----");
        /*原始方法执行*/
        Object obj = joinPoint.proceed();
        return obj;

    }

}
~~~

~~~xml
<bean id="userSerivce" class="org.example.aspect.UserServiceImpl"></bean>
    <!--切面对象
            包含额外功能
            切入点
            组装细节
            -->
    <bean id="aspect" class="org.example.aspect.MyAspect"></bean>
    <!--    告知是基于注解的AOP编程-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
~~~

####  2.细节

##### 2.1切入点复用

~~~java
 @Pointcut("execution(* login(..))")/*复用的函数，需要@Pointcut注解*/
    public void myPointCut(){}


    @Around(value = "myPointCut()")/*标注切入点*/
    /*等同invoke方法*/
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("----log-----");
        /*原始方法执行*/
        Object obj = joinPoint.proceed();
        return obj;

    }
~~~

##### 2.2代理模式

~~~markdown
* AOP底层实现 2中代理模式
	* JDK代理模式。通过实现接口 创建实现类 创建代理对象
	* Cglib通过继承父类， 做新的子类 创建代理对象
	
* 默认的情况是基于JDK的动态代理
	* 如果想要换成Cglib，需要在xml配置文件中做出属性指定
	基于注解的切换
		<aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
	传统的AOP开发
		<aop:config proxy-target-class="true"></aop:config>
~~~

