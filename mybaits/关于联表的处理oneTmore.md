关于数据库表的设计：

![](E:\MarkdownPicture\mydb.png)

实体类代码

```java
package cn.sxt.entity;

public class Student {
	private int id;
	private String name;
	private Teacher teacher;
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", teacher=" + teacher + "]";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Teacher getTeacher() {
		return teacher;
	}
	public void setTeacher(Teacher teacher) {
		this.teacher = teacher;
	}
	

}

```

```java 
package cn.sxt.entity;

public class Teacher {
	private int id;
	private String name;
	@Override
	public String toString() {
		return "Teacher [id=" + id + ", name=" + name + "]";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}

}

```

编写查询的映射文件：

按照结果嵌套

mybaits-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

	<typeAliases>
		<package name="cn.sxt.entity"/>
	</typeAliases>
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
	 	<mapper resource="cn/sxt/entity/student.mapper.xml"/><!-- 导入mapper文件 -->
	</mappers>

</configuration>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.student.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
		<!-- 多对一的查询方式：
		1.按结果嵌套处理
		2.按查询嵌套处理
		 -->
		 
		 
		 <!--按结果嵌套处理-->
	<select id="getStudent" resultMap="StudentTeacher">
		select s.id as sid,s.name as sname,s.tid as stid,t.id  as tid ,t.name as tname  from student s,teacher t where s.tid=t.id
	</select>
	<resultMap type="Student" id="StudentTeacher">
		<id column="sid" property="id"/>
		<result column="sname" property="name"/>
		<association property="teacher" javaType="Teacher">
			<id column="tid" property="id"/>
			<result column="tname" property="name"/>
		</association>
	</resultMap>
</mapper>
```

```

```



```java
package cn.sxt.utils;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class MybatisUtil {
	//创建SqlSessionFactory
	public static SqlSessionFactory getSqlSessionFactory() throws IOException {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactory sqlSessionFactory =
		 new SqlSessionFactoryBuilder().build(inputStream);
		return sqlSessionFactory;
	}
	//创建SqlSession
	public static SqlSession getSqlSession() throws IOException {
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		return sqlSessionFactory.openSession();
		
	}
}

```



```java 
package cn.sxt.dao;

import java.io.IOException;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

import cn.sxt.entity.Student;
import cn.sxt.utils.MybatisUtil;

public class StudentDao {
	public List<Student> getAll() throws IOException {
		SqlSession session = MybatisUtil.getSqlSession();
		List<Student> students = session.selectList("cn.sxt.entity.student.mapper.getStudent");
		session.close();
		return students;
	}

}

```

```java
package cn.sxt.entity;

public class Student {
	private int id;
	private String name;
	private Teacher teacher;
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", teacher=" + teacher + "]";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Teacher getTeacher() {
		return teacher;
	}
	public void setTeacher(Teacher teacher) {
		this.teacher = teacher;
	}
	

}

```

```java
package cn.sxt.entity;

public class Teacher {
	private int id;
	private String name;
	@Override
	public String toString() {
		return "Teacher [id=" + id + ", name=" + name + "]";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}

}

```

```java 
package cn.sxt.test;

import java.io.IOException;
import java.util.List;
import cn.sxt.dao.StudentDao;
import cn.sxt.entity.Student;


public class Test {

	public static void main(String[] args) throws IOException {
		StudentDao studentDao = new StudentDao();
		List<Student> students = studentDao.getAll();
		for (Student student : students) {
			System.out.println(student.toString());
		}
		
	}

}

```

相对的查询嵌套只是改变了映射文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.teacher.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
		<!-- 多对一的查询方式：
		1.按结果嵌套处理
		2.按查询嵌套处理
		 -->
		 <!--按结果嵌套处理
	<select id="getStudent" resultMap="StudentTeacher">
		select s.id as sid,s.name as sname,s.tid as stid,t.id  as tid ,t.name as tname  from student s,teacher t where s.tid=t.id
	</select>
	<resultMap type="Student" id="StudentTeacher">
		<id column="sid" property="id"/>
		<result column="sname" property="name"/>
		<association property="teacher" javaType="Teacher">
			<id column="tid" property="id"/>
			<result column="tname" property="name"/>
		</association>
	</resultMap>
	-->
	 <!--按查询嵌套-->
	 <select id="getTeacher" resultType="Teacher">
	 select * from teacher where id=#{id}
	 </select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.student.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
		<!-- 多对一的查询方式：
		1.按结果嵌套处理
		2.按查询嵌套处理
		 -->
		 <!--按结果嵌套处理
	<select id="getStudent" resultMap="StudentTeacher">
		select s.id as sid,s.name as sname,s.tid as stid,t.id  as tid ,t.name as tname  from student s,teacher t where s.tid=t.id
	</select>
	<resultMap type="Student" id="StudentTeacher">
		<id column="sid" property="id"/>
		<result column="sname" property="name"/>
		<association property="teacher" javaType="Teacher">
			<id column="tid" property="id"/>
			<result column="tname" property="name"/>
		</association>
	</resultMap>
	-->
	 <!--按查询嵌套-->
	 <select id="getStudent" resultMap="StudentTeacher">
	 select * from student
	 </select>
	 <resultMap type="Student" id="StudentTeacher">
	 	<association property="teacher" column="tid" javaType="Teacher" select="cn.sxt.entity.teacher.mapper.getTeacher"></association>
	 </resultMap>

	 
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

	<typeAliases>
		<package name="cn.sxt.entity"/>
	</typeAliases>
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
	 	<mapper resource="cn/sxt/entity/student.mapper.xml"/><!-- 导入mapper文件 -->
	 	<mapper resource="cn/sxt/entity/teacher.mapper.xml"/><!-- 导入mapper文件 -->
	</mappers>

</configuration>
```

