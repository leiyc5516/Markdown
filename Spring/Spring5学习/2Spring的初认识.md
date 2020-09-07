### 第二章 第一个Spring程序

#### 1.版本

```
1. JDK1.8+
2. Maven3.5+
3. IDEA 2018+
4. SpringFramework5.1.4+
```

#### 2.环境搭建

1. spring的jar

   ````xml
   1.设置pom.xml 依赖文件
      <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.1.14.RELEASE</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.1.14.RELEASE</version>
       </dependency>
   ````

2. Spring配置文件

   ```markdown
   1.放置位置：任意位置没有硬性要求
   2.配置文件的命名：没有硬性要求 建议使用applicationContext.xml
   3.日后使用Spring框架时，需要设置配置文件的路径
   ```

   ![image-20200728153020260](E:\MarkdownPicture\image-20200728153020260.png)

3. Spring的核心API

   * ApplicationContext

   ```markdown
   ApplicationContext作用：Spring这个工厂主要是解耦合，用于对象的创建
   ```

   * ApplicationContext接口类型

   ```markdown
   接口主要是为了屏蔽掉实现差异
   1.非web环境下的工厂：ClassPathXmlApplicationContext(main junit)
   2.web环境下的工厂 ： XmlWebApplicationContext
   ```

   

![image-20200728160257024](E:\MarkdownPicture\image-20200728160257024.png)

4. 重量级的资源

~~~markdown
1.ApplicationContext工厂的对象占用大量资源内存
2.不会频繁的创建对象 ： 一个应用只会创建一个工厂对象
3.ApplicationContext工厂：一定是线程安全的[多线程访问]
~~~

#### 3.程序开发

~~~markdown
1.创建类型
2.配置文件的配置 applicationContext.xml
3.通过工厂类获得对象
    ApplicationContext
    |-ClassPathxmlApplicationContext
~~~

~~~java 
public class Person {
   String name;
   int age;
}
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
    <!-- id属性 名字 （唯一）
         class属性 配置全限定名
    -->
    <bean id="person" class="org.example.Person"></bean>
</beans>
~~~



获取类的常见的方法

~~~java
//获取Spring的工厂
ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
//通过工厂获取对象
Person person = (Person) ctx.getBean("person");
~~~

~~~Java
 //获取Spring的工厂
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
//通过工厂获取对象
Person person = ctx.getBean("person",Person.class);
~~~

~~~Java
 //获取Spring的工厂
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
//通过工厂获取对象
Person person = ctx.getBean(Person.class);
~~~

一些特殊的情况，例如一个类多个名字

* **细节之常用方法**

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
    <!-- id属性 名字 （唯一）
         class属性 配置全限定名
    -->

    <bean id="person" class="org.example.Person"></bean>
    <bean id="personnew" class="org.example.Person"></bean>
</beans>
~~~

~~~Java
 public void test4(){
        //获取Spring的工厂
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //获取所有的名称
        String[] beanDefinitionNames =ctx.getBeanDefinitionNames();
        for (String name: beanDefinitionNames
             ) {
            System.out.println(name);
        }
    }
~~~

~~~Java
 public void test5(){
        //获取Spring的工厂
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //获取该类型的Bean名称
        String[] Names =ctx.getBeanNamesForType(Person.class);
        for (String name: Names
        ) {
            System.out.println(name);
        }
    }
~~~

~~~Java
    @Test
    public void test6(){
        //获取Spring的工厂
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //是否包含这个Bean类的
        boolean flag = ctx.containsBean("person");
       System.out.println(flag);
    }
~~~

~~~java
    @Test
    public void test7(){
        //获取Spring的工厂
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //是否包含这样的一个Bean
        boolean flag = ctx.containsBeanDefinition("person");
        System.out.println(flag);
    }
~~~



*  **细节之配置文件 **

~~~xml
1.在xml文件中只配置class属性不配置ID
<bean class=""></bean>
a)上述配置包含隐含的ID
b)如果Bean使用多次或者被引用要配置id
2.使用name属性定义别名，作用类似于ID属性
<bean name="p" class="org.example.Person"></bean>
3.ID与Name属性的区别与相同点
相同点：两者都可以区分Bean
不通点：name可以定义多个，但是ID只可以定义一个；xml中ID属性值命名需要字母开头，name可以使用下划线反斜线（/）开头这个时候可以使用name代替ID。当下版本的xml已经不存在上述的问题。
4.代码的区别
 在常用方法中containsBeanDefinition只可以判断ID不可以判断name
 而对于containsBean两者都可以判断
~~~

#### 4.Spring工厂底层实现（简易版）

![image-20200729093712069](E:\MarkdownPicture\image-20200729093712069.png)

#### 5.思考

~~~markdown
问题：开发过程中是否所有的对象都需要交给Spring工厂来创建？
回答：理论上确实是，但是实体（entity）的创建需要交给持久层创建的。
~~~

