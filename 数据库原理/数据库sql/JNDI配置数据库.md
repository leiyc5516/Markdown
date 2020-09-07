
* * *
>作者：菜鸟kenshine
链接：[点这里](https://www.jianshu.com/p/de936f8ce3a9)
来源：简书
>**JNDI的简介及优点：**
>>1.  JNDI的定义：
    上面知道了JNDI的作用，JNDI的定义也就很容易理解了：
    JNDI是Java命名与文件夹接口（Java Naming and Directory Interface），在J2EE规范中是重要的规范之中的一个。
    简单的理解，区别于JDBC,就是：
    jdbc是java去找数据库驱动，jndi是通过你的服务器配置（如Tomcat）的配置文件context来找数据库驱动~
>>2.  现在JNDI已经成为J2EE的标准之一，所有的J2EE容器都必须提供一个JNDI的服务。(web容器就是Tomcat等服务器充当的)
>>3.  为什么使用了连接池还要用JNDI：
    > 为了数据库资源的管理，在容器中配置一个数据库连接池，使用JNDI 来管理
    > 这样容器中运行多个服务的时候，每个服务只需添加一个jndi的名称就可以连接到数据库了
    > 如果不使用jndi的方式，直接在项目中配置数据库连接池，那么每个项目需要配置一次，如果更改数据库地址时，
    > 每个项目的数据库连接方式都要更改，比较麻烦，使用jndi的话，直接更改一下jndi里面的数据库连接池的配置就可以了，方便一些。
>>4.  一般来说如果目标客户有专业的应用服务器，比如 WebSphere，WebLogic，我们就不需要在代码中配置使用特定的 dbcp 或其它的连接池了。只使用JNDI就可以了。

***
```
 /*
* 创建JNDI上下文获取对象
*/
 /*
 * 查询入口
*/
            InitialContext initCtx = new InitialContext();//初始化上下文查找入口
        	Context  envCtx= (Context)initCtx.lookup("java:emp/env");
        	//二次查找
        	DataSource dataSource =  (DataSource) envCtx.lookup("jdbc/mysql");//使用的名称与Resource的元素<name>一致
        	Connection con = dataSource.getConnection();//得到连接

```
###待深入


