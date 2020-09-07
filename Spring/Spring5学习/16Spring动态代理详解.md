### 第十六章，Spring动态代理详解

#### 1. Spring动态代理开发四个基本步骤

#### 开发步骤

1. 创建原始对象（目标对象）

   ~~~java
   public class UserServiceImpl implements UserService {
   
       @Override
       public void register(org.example.staticproxy.User user) {
           System.out.println("业务运算以及所谓的DAO调用操作");
           
       }
   
       @Override
       public boolean login(User user) {
           System.out.println("完成核心的业务运算");
           return true;
       }
   }
   ~~~

   ~~~xml
   <bean id="userSerivce" class="org.example.dynomicproxy.UserServiceImpl"></bean>
   ~~~

2. 创建额外功能

   MethodBeforeAdvice接口

   ~~~markdown
   1.额外的功能书写在接口实现中，运行在原始方法运行前。
   ~~~

   ~~~java
   public class Before implements MethodBeforeAdvice {
       //把需要运行的在原始方法之前的方法书写在这个方法中
       @Override
       public void before(Method method, Object[] objects, Object o) throws Throwable {
           System.out.println("-----method before----");
       }
   }
   ~~~

   ~~~xml
   <bean id="userSerivce" class="org.example.dynomicproxy.UserServiceImpl"></bean>
   <bean id="before" class="org.example.dynomicproxy.Before"></bean>
   ~~~

   

3. 定义切入点

   ~~~markdown
   * 额外功能加入的位置
   * 目的：由程序员根据自己的需求自己额外功能加入到那个原始方法
   ~~~

   ~~~xml
   <bean id="userSerivce" class="org.example.dynomicproxy.UserServiceImpl"></bean>
   <bean id="before" class="org.example.dynomicproxy.Before"></bean>
       <aop:config >
           <aop:pointcut id="pc" expression="execution(* *(..))"/>
       </aop:config>
   
   ~~~

   

4. 组装（将第二步 和第三步整合）

   ~~~xml
   <aop:config >
           <aop:pointcut id="pc" expression="excution(* *(..))"/>
       <!--将代理类和需要切入的点，使用代理的类做组装-->
           <aop:advisor advice-ref="before" pointcut-ref="pc"></aop:advisor>
   </aop:config>
   ~~~

5. 调用

   ~~~markdown
   * 获取Spring工厂创建代理对象，并进行调用
   * Spring 工厂通过原始对象id获取的对象是代理对象
   ~~~

#### 2.详解额外功能

* MethodBeforeAdvice分析
* 额外功能需要运行在原始方法执行之前，进行额外功能操作

~~~java
public class Before implements MethodBeforeAdvice {
    //把需要运行的在原始方法之前的方法书写在这个方法中
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("-----method before----");
    }
}
~~~

~~~markdown
* 👆诉代码详解
* Method:额外功能增加给的那个原始方法     Method是变化的
* Object[]：额外功能所增加的那个原始方法的参数，String name,String password.与Method方法的参数息息相关
* Object：额外功能所增加的那个原始对象 
		
~~~

* before方法的三个参数在实战中，该如何使用。

  ~~~markdown
   * 根据实战需要决定是否使用，并不一定适用
  ~~~

#### 3.MethodInterfaceptor(方法拦截器)相对的是比MethodBeforeAdvice功能强大

~~~markdown
* MethodInterfaceptor接口中的方法可以根据需求将额外功能加到核心方法  **前后**，也可以运行在额外功能抛出异常的时候
~~~



~~~java
package org.example.dynomicproxy;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

public class Around implements MethodInterceptor {
    /*
        invoke方法作用：额外功能书写在invoke方法中
        额外功能      ：    原始方法之前
                    ：    原始方法之后
                    ：     原始方法之前    原始方法之后
        确定原始方法：原始方法怎么运行
        MethodInvocation：额外功能所增加给那个原始方法
        methodInvocation：具体到每个调用代理的原始方法
        返回值Object ：原始方法执行后的返回值，是跟原始方法相关
     */

    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        /*
            该方法可以让原始方法运行
         */
        System.out.println("原始方法之前");
         Object ret = methodInvocation.proceed();
        System.out.println("原始方法之后");
         return ret;
    }
}

~~~

~~~xml
    <bean id="userSerivce" class="org.example.dynomicproxy.UserServiceImpl"></bean>
    <bean id="around" class="org.example.dynomicproxy.Around"></bean>
    <aop:config >
        <aop:pointcut id="pc" expression="execution(* *(..))"/>
        <aop:advisor advice-ref="around" pointcut-ref="pc"></aop:advisor>
    </aop:config>
~~~

* MethodInterfaceptor影响原始方法的返回值

~~~markdown
* 原始方法的返回值，不做修改直接返回就可以。这样是不做修改的结果。
* 如果想要影响原始方法的值，直接可以在MethodInterceptor的invoke中做出修改返回就可以
~~~

