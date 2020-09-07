配置文件配置优化

1.关于Mybaits-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!-- 根节点文件 -->
	<environments default="development">
	<!-- 
	environments指的是可以配置多个环境  default 指向默认的环境
	每个环境都会创建对应的 SqlSessionFactory
	-->
	 <environment id="development">
	 	<!-- MyBatis的事务管理分为两种形式：

		（1）使用JDBC的事务管理机制。
		
		这种机制就是利用java.sql.Connection对象完成对事务的提交
		
		（2）使用MANAGED的事务管理机制。
		
		这种机制mybatis自身不会去实现事务管理，而是让程序的Web容器或者Spring容器来实现对事务的管理。
		 -->
	 	<transactionManager type="JDBC"/>
	 	
	 	<!-- 
	 	数据库源类型：
	 	UNPOOLED：需要的时候打开或关闭
	 	POOLED：使用数据库连接池
	 	 -->
		 <dataSource type="POOLED">
		 	<property name="driver" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
			<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
			<property name="username" value="root"/><!-- 设置用户的值 -->
		 	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
		 </dataSource>
	 </environment>
	</environments>
	<!-- 定义了映射SQL语句的文件 -->
	<mappers>
	 	<mapper resource="cn/sxt/entity/user.mapper.xml"/><!-- 导入mapper文件 -->
	</mappers>

</configuration>
```

MyBatis的事务管理分为两种形式：

（1）使用JDBC的事务管理机制。

这种机制就是利用java.sql.Connection对象完成对事务的提交

（2）使用MANAGED的事务管理机制。

1.这种机制mybatis自身不会去实现事务管理，而是让程序的Web容器或者Spring容器来实现对事务的管理。

2.关于SQL语句映射的mapper文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
 	<select id="selectUser" resultType="cn.sxt.entity.User"><!--id 作为select辨识符号标志着每个SQL语句防止重覆 resultType="包名和类名" -->
 		select * from user where id = #{id}<!-- sql语句，#{注入的值} -->
 	</select>
 	<insert id="insertUser" parameterType="cn.sxt.entity.User" useGeneratedKeys="true">
 		insert into user(id,name,password) values(#{id},#{name},#{password})
 	</insert>
 	<update id="updateUser" parameterType="cn.sxt.entity.User" >
 		update user set name=#{name},password=#{password} where id=#{id}
 	</update>
 	<delete id="deleteUser">
 		delete from user where id=#{id}
 	</delete>
</mapper>
```

MyBatis 的真正强大在于它的映射语句，也是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 就是针对 SQL 构建的，并且比普通的方法做的更好。 

3.执行流程：

读取核心配置文件--->创建SqlSessionFactory---->获取SqlSession----->(包含对数据库的所有的操作方法，执行数据库的操作方法 )

4.优化配置文件

导入properties配置文件,并在mybaits-comfig.xml配置 

```xml
<properties resource="db.properties"></properties>
```

```xml
 <dataSource type="POOLED">
		 	<property name="driver" value="${driver}"/><!-- 设置驱动的值 -->
			<property name="url" value="${url}"/><!-- 设置数据库值 -->
			<property name="username" value="${username}"/><!-- 设置用户的值 -->
		 	<property name="password" value="${password}"/><!-- 设置密码的值 -->
		 </dataSource>


```

配置别名：这样可以简化mapper.xml的编写

```xml
<typeAliases>
 	<typeAlias type="cn.sxt.entity.User" alias="User"/>
</typeAliases>
```





