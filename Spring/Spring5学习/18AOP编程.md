### 第十八章，AOP

#### 1.AOP概念

~~~markdown
* AOP (Aspect Oriented Programing)面向切面编程
* 以切面作为基本单位的程序开发，通过对间彼此协同，相互调用，完成程序的构建
* 切面 = 切入点 + 额外的功能
* AOP本质：
	本质就是Spring的动态代理开发，通过代理原始类增加额外功能
	好处：利于原始类的维护
* 注意：有了AOP编程 不可以取代OOP编程 ，是对OOP编程的补充
~~~

#### 2.AOP编程的开发步骤

~~~markdown
* 原始对象
* 额外功能
* 切入点
* 组装切面（额外功能 + 切入点）
~~~

#### 3.切面的名词解释

~~~markdown
* 切面 = 切入点 + 额外功能
~~~

![image-20200811153440711](E:\Markdown\Spring\Spring5学习\image-20200811153440711.png)



#### 4.AOP底层实现原理

~~~markdown
* AOP如何创建动态代理类（动态字节码技术）
* Spring工程如何加工创建代理对象
	通过原始对象的id,获取对象的具体底层实现 
~~~

~~~markdown
* JDK的动态代理
	通过实现相同接口实现功能相同
	
* CGLIB的动态代理
	通过继承的方式把原始类的方法继承下来并添加相关额外功能
~~~

##### 4.1JDK动态代理

<img src="E:\Markdown\Spring\Spring5学习\image-20200811160949795.png" alt="image-20200811160949795" style="zoom:200%;" />

<img src="E:\Markdown\Spring\Spring5学习\image-20200811161037956.png" alt="image-20200811161037956" style="zoom:200%;" />

~~~java
package org.example.jdk;

import org.example.dynomicproxy.UserServiceImpl;
import org.example.staticproxy.UserService;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class TestJdkProxy {
    public static void main(String args[]){
        org.example.staticproxy.User user = new org.example.staticproxy.User();
        user.setName("aa");
        user.setAge(10);
        //创建原始对象
        UserService userService = (UserService) new UserServiceImpl();
        //jdk动态创建代理
        InvocationHandler invocationhandler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //原始对象方法运行
                System.out.println("-----log-----");
                Object ret = method.invoke(userService,args);
                return ret;
            }
        };
        Proxy.newProxyInstance(TestJdkProxy.class.getClassLoader(),userService.getClass().getInterfaces(),invocationhandler);
        userService.login(user);
    }

}

~~~

#### 4.2CGlIB创建动态代理

~~~markdown
* CGLIB通过动态代理：父子继承关系创建代理对象，原始类作为父类，代理类作为子类，这样就可以保证两者的方法一致，同时在代理类中提供行的功能，又可以保留原来的方法
~~~



<img src="E:\Markdown\Spring\Spring5学习\image-20200811163749587.png" alt="image-20200811163749587" style="zoom:200%;" />

##### 开发步骤

~~~java
package org.example.cglib;

import org.example.jdk.TestJdkProxy;
import org.example.staticproxy.UserServiceProxy;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * @Author: Colin
 * @Date: 2020/8/11 16:43
 * @Version: 1.0
 * @Description:
 */
public class TestCglib {
    public static void main(String args[]) {
        //创建原始对象
        UserService userService = new UserService();
        /*
        2通过cglib方式动态创建代理对象
        Proxy.newProxyInstance(classloader,interfaces,invocationhanlder)

        Enhancer.setClassLoader()设置类加载器
        Enhancer.setSuperClass()设置父类
        Enhancer.setCallback()----->MethodInterceptor(cglib)编写额外功能
        Enhancer.create()---->创建代理
        */
        Enhancer enhancer = new Enhancer();
        enhancer.setClassLoader(TestCglib.class.getClassLoader());
        enhancer.setSuperclass(userService.getClass());
        MethodInterceptor methodInterceptor = new MethodInterceptor() {
            @Override
            public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                System.out.println("-----log-----");
                Object ret = method.invoke(userService,objects);
                return ret;
            }
        };
        enhancer.setCallback(methodInterceptor);
        UserService userServiceproxy = (UserService) enhancer.create();
        userServiceproxy.login("suns","ass");
        userServiceproxy.register("suns","aaa");
    }
}

~~~



#### 总结

~~~markdown
* JDK动态代理 Proxy.newProxyInstance() 通过接口创建代理的实现类
* Cglib动态代理 Enhancer 通过继承父类创建代理类
~~~

#### 5.Spring工厂如何加工原始的对象，BeanPostProcessor

<img src="E:\Markdown\Spring\Spring5学习\image-20200811171829191.png" alt="image-20200811171829191" style="zoom:100%;" />

~~~java
package org.example.factory;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class ProxyBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("----new log----");
                Object ret = method.invoke(bean,args);
                return ret;
            }
        };
        return Proxy.newProxyInstance(ProxyBeanPostProcessor.class.getClassLoader(),bean.getClass().getInterfaces(),handler);
    }
}

~~~

~~~xml
 <bean id="userService" class="org.example.factory.UserServiceImpl"></bean>
        <bean id="proxyBeanPostProcessor" class="org.example.factory.ProxyBeanPostProcessor"></bean>
~~~

