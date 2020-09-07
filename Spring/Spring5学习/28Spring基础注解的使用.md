### 第十八章，Spring基本注解的使用

~~~markdown
1.在spring2.x使用的都是简化xml的一些注解
~~~

#### 1.创建相关注解

* 搭建开发环境

~~~xml
<context:component-scan base-package="com.leiyc"></context>
作用是：让Spring框架扫描注解，扫描包和子包。。。。
~~~

* 对象创建相关注解

  * @Conponent :适用于替换原有Spring配置文件的Bean 标签。默认id体现就是类的首单词首字母小写

    ![image-20200821222356782](E:\Markdown\Spring\Spring5学习\image-20200821222356782.png)

* @conponent(“id”)指定ID
* spring配置文件覆盖注解内容

~~~xml
applicationcontext.xml
<bean id="user" class="com.leiyc.bean.User"></bean>
~~~

* @Conponent衍生注解：

~~~markdown
* @Conponent都可以用
* @Repository dao层
* @Service Service层
* @Controller Controller层
上述注解和@Conponent注解基本一致
为了表达不同的类型层的对象创建
~~~

* @Scope控制简单对象创建的次数

~~~markdown
@Scope("singleton") 使用只创建一次
@Scope("prototype") 使用的时候创建多次
~~~

* @Lazy作用延时创建单实例对象

~~~markdown
一旦使用@Lazy注解后，Spring会在使用这个对象的创建
~~~

* 生命周期方法的注解

~~~markdown
* 初始化方法 @PostConstruct
	* Initialzingbean
	<bean init-method="">
* 销毁方法  @PreDestroy
	* DisposableBean
	<bean destroy-method="">
~~~

