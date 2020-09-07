### 第二十一章，AOP阶段总结

~~~markdown
* AOP编程的概念（Spring的动态代理开发）
* 概念 ：通过代理类为原始的类增肌额外功能
* 好处：利于原始类的维护
~~~

~~~markdown
* AOP开发步骤（动态代理开发）
	* 原始对象
	* 额外功能
	* 切入点
	* 组装切面
~~~

~~~markdown
* AOP编程方式
	* 传统的方式
		* 实现MethodInterceptor接口并在invoke方法中实现额外功能和原始方法调用。
		* 在xml中组装切面:
		```<aop:config >
        ```<aop:pointcut id="pc" expression="execution(* login(..))"/>
        ```<aop:advisor advice-ref="around" pointcut-ref="pc"></aop:advisor>
    	```</aop:config>
	* 基于注解的AOP编程
		* java
        ```@Aspect
        ```public class MyAspect {
        ```Pointcut("execution(* login(..))")
        ```public void myPointCut(){}
        ```@Around(value = "myPointCut()")
        ```/*等同invoke方法*/
        ```public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        ```System.out.println("----log-----");
        ```/*原始方法执行*/
        ```Object obj = joinPoint.proceed();
        ```return obj;
        `````}
        ```}
       * xml
       	```<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
		
~~~

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

