依赖注入DI：

Dependency injection

依赖：指的是Bean对象创建是依赖于容器，Bean对象依赖资源是可以使配置文件，对象，基本数据类型等

注入：Bean对象依赖的资源由容器来装配和设置。

> **Spring中的注入：**

> > 1）构造方法注入
> >
> > > 见IOC创建对象
> > >
> > > > 有参构造
> > > >
> > > > ```xml
> > > > <bean id="user1" class="cn.leiyc.vo.User">
> > > > 		<!-- index标记第几个参数，value标注值 -->
> > > > 		<constructor-arg index="0" value="李四"></constructor-arg>
> > > > </bean>
> > > > ```
> > >
> > > C注入：
> > >
> > > >```xml
> > > >xmlns:c="http://www.springframework.org/schema/c"
> > > >```
> > > >
> > > >```xml
> > > ><bean id="address2" class="com.baidu.pojo.Address" c:name="宝鸡市"/>
> > > ><!--有参构造方法注入-->
> > > >```
> > > >
> > > >
> >
> > 2）**set方法注入**
> >
> > > 要求被注入的属性必须有set方法**set+属性首字母大写**如果属性是boolean类型没有get方法而是is方法
> > >
> > > > 常量注入：
> > > >
> > > > ```xml
> > > >    <bean id="student" class="cn.leiyc.vo.Student">
> > > >    <!-- id创建的实例的名字，class需要反射创建的类名 -->
> > > >    	<property name="name" value="雷宁"></property>
> > > >    	<!-- name需要设置的属性的set方法后面首字母小写的单词，value为数值-->
> > > >    </bean>
> > > > ```
> > > >
> > > > Bean对象注入：
> > > >
> > > > ```xml
> > > > 	<bean id="Mysql" class="cn.leiyc.dao.impl.UserMysqlImpl"> 
> > > > 	</bean>
> > > > 	<bean id="Oracle" class="cn.leiyc.dao.impl.UserOracleImpl"> 
> > > > 	</bean>
> > > > 	<bean id="service" class="cn.leiyc.service.impl.UserServiceImpl">
> > > > 		<property name="userDao" ref="Mysql"></property>
> > > > 		<!-- name值为set方法后面的名称首字母小写，ref后面是Bean对象实例化的ID或者说对象名 -->
> > > > 	</bean>
> > > > ```
> > > >
> > > > 数组注入：
> > > >
> > > > ```xml 
> > > >     <bean id="student" class="cn.leiyc.vo.Student">
> > > >    <!-- 注入数组 -->
> > > >    	<!-- name需要设置的属性的set方法后面首字母小写的单词，value为数值-->
> > > >    	<property name="books">
> > > >    		<array>
> > > >    			<value>傲慢与偏见</value>
> > > >    			<value>简爱</value>
> > > >    			<value>人间</value>
> > > >    		</array>
> > > >    	</property>
> > > >    </bean>
> > > > ```
> > > >
> > > > List注入：
> > > >
> > > > ```xml
> > > >    <bean id="student" class="cn.leiyc.vo.Student">
> > > >    	<!-- 注入List -->
> > > >    	<property name="gilrFriends">
> > > >    		<list> 
> > > >    			<value>lisa</value>
> > > >    			<value>marrie</value>
> > > >    			<value>lily</value>
> > > >    		</list>
> > > >    	</property>
> > > >    </bean>
> > > > ```
> > > >
> > > > Set注入：
> > > >
> > > > ```xml
> > > >    <bean id="student" class="cn.leiyc.vo.Student">
> > > >    	<!-- 注入List -->
> > > >    	<property name="gilrFriends">
> > > >    		<set> 
> > > >    			<value>lisa</value>
> > > >    			<value>marrie</value>
> > > >    			<value>lily</value>
> > > >    		</set>
> > > >    	</property>
> > > >    </bean>
> > > > ```
> > > >
> > > > 
> > > >
> > > > null注入：
> > > >
> > > > ```xml
> > > >    <bean id="student" class="cn.leiyc.vo.Student">
> > > >    	<!-- 注入List -->
> > > >    	<property name="gilrFriends">
> > > >    		<null></null>
> > > >    	</property>
> > > >    </bean>
> > > > ```
> > > >
> > > > properties注入：
> > > >
> > > > ```xml
> > > >    
> > > >     <bean id="student" class="cn.leiyc.vo.Student"><property name="info">
> > > >    		<props>
> > > >    			<prop key="学号">55160831</prop>
> > > >    			<prop key="成绩">89.45</prop>
> > > >    			<prop key="grand">skkd</prop>
> > > >    		</props>
> > > >    	</property>
> > > >    	</bean>
> > > > ```
> > > >
> > > > 
> >
> > 3）P名称空间注入（需要添加xml文件约束头）
> >
> > ```xml
> > xmlns:p="http://www.springframework.org/schema/p" 
> > ```
> >
> > 
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8"?>
> > <beans xmlns="http://www.springframework.org/schema/beans"
> >     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
> >     xmlns:p="http://www.springframework.org/schema/p"
> >     xsi:schemaLocation="
> >         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
> > 
> >      <bean id="user" class="com.test.spring.aop.User"></bean>
> >     <bean id="userDao" class="com.test.spring.aop.UserDao"></bean>
> >     <bean id="userService" class="com.test.spring.aop.UserService">
> >         <property name="userName" value="小王"></property>
> >         <property name="userDao" ref="userDao"></property>
> >     </bean> 
> >     <!--P名空间注入-->
> >     <bean id="userService" class="com.leiyc.UserService" 
> >           p:userName="小王" p:userDao-ref="userDao">
> >     </bean>
> > </beans>
> > ```
> >
> > ```xml
> > <bean id="address1" class="com.baidu.pojo.Address" p:name="陕西省"/>
> > <!--属性注入-->
> > ```
> >
> > 
> >
> > 4）SpEL注入

> Bean的作用域：
>
> Scope：当通过spring容器创建一个Bean实例时，不仅可以完成Bean实例的实例化，还可以为Bean指定特定的作用域。Spring支持如下6种作用域：
>
> > singleton：单例模式，在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例
>
> > prototype：原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例
>
> > request：对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效
>
> > session：对于每次HTTP Session，使用session定义的Bean豆浆产生一个新实例。同样只有在Web应用中使用Spring时，该作用域才有效
>
> > global session：每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效
>
> > application ：在应用的范围中一个对象

自动装配Bean：

为了缩减配置文件

```xml
<bean id="user1" class="cn.leiyc.vo.User"></bean>
<!--autowire="byName" 或则是 autowire="byType"按照类型或者名字来装配，ByName根据属性的set方法名称来设置，ByType通过类型自动装配-->
<bean id = "factory" class="cn.leiyc.factory.UserDynamicFactory" autowire="byName"></bean>
```



