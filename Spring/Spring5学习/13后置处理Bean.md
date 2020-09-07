### 第十三章，后置处理Bean

~~~markdown
BeanPostProcessor 作用：对Spring工厂所创建的对象，进行加工
AOP的底层实现的很多都需要BeanPostProcessor技术的支持
~~~

~~~java
BeanPostProcesser接口中{
    //接口方法
    xxxx(){
        //加工的方法
    }
}
~~~

![创建对象流程图](E:\Markdown\Spring\Spring5学习\image-20200810141628322.png)

![image-20200810142020742](E:\Markdown\Spring\Spring5学习\image-20200810142020742.png)

 后置Bean处理的处理过程

~~~java 
需要对BeanPostProcessor接口中方法实现
    //作用：Spring创建完对象，并进行注入的时候，可以运行postProcessBeforeInitialization（）加工
    //获得Spring创建的对象:通过方法参数
    //最终通过返回值交还给Spring框架
       @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return Bean;
    }
	//作用：Spring初始化完对象，可以运行postProcessAfterInitialization（）加工
    //获得Spring创建的对象:通过方法参数
    //最终通过返回值交还给Spring框架

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return Bean;
    }

//实战上：很少处理Spring的初始化操作，没有区分 Before After，只需要实现其中一个After方法即可，但是必须空实现Before,实现After
~~~

* #### 对BeanPostProcessor开发步骤

##### 1. 类的实现BeanPostProcessor接口

~~~java
package org.example.beanpost;

public class Categroy {
    private int age ;
    private String name;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Categroy{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
//这是一个Bean类
~~~



~~~java

public class MyBeanPostProcesser implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        //增加类型判断
    if (bean instanceof Categroy) {
        Categroy categroy = (Categroy) bean;
        categroy.setName("消亡");
    }
        return bean;
    }
}
//这是一个加工类
~~~



##### 2.Spring的配置文件中进行配置

~~~xml
<bean id="categroy" class="org.example.beanpost.Categroy">
            <property name="age" value="20"></property>
            <property name="name" value="leiyc"></property>
</bean>
<bean id="myBeanPostProcessor" class="org.example.beanpost.MyBeanPostProcesser"></bean>
~~~

* 细节

  ~~~markdown
  	BeanPostProcessor接口会对所有Spring创建的的Bean对象加工
  ~~~

  