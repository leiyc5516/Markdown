![](E:\MarkdownPicture\mydb.png)

实体类

```java
package cn.sxt.entity;

import java.util.List;

public class Teacher {
	private int id;
	private String name;
	private List<Student> students;
	public List<Student> getStudents() {
		return students;
	}
	public void setStudents(List<Student> students) {
		this.students = students;
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
	@Override
	public String toString() {
		return "Teacher [id=" + id + ", name=" + name + ", students=" + students + "]";
	}
	

}

```

```java
package cn.sxt.entity;

import java.util.List;

public class Student {
	private int id;
	private String name; 
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + "]";
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

编写映射文件：

一对多实现的两种方式：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.teacher.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
	<select id="getTeacher" resultMap="TeacherStudent">
		select s.id sid,s.name sname,s.tid stid,t.id tid ,t.name tanme  from student s,teacher t where s.tid=t.id and t.id=#{id}
	</select>
	<resultMap type="Teacher" id="TeacherStudent">
		<id column="tid" property="id"/>
		<result column="tname" property="name"/>
		<collection property="students" ofType="Student"><!-- oftype -->
			<id column="sid" property="id"/>
			<result column="sname" property="name"/>
		</collection>
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

```java
package cn.sxt.dao;

import java.io.IOException;

import org.apache.ibatis.session.SqlSession;
import cn.sxt.entity.Teacher;
import cn.sxt.utils.MybatisUtil;

public class TeacherDao {
	public Teacher getTeacher(int id) throws IOException {
		SqlSession session = MybatisUtil.getSqlSession();
		Teacher teacher = session.selectOne("cn.sxt.entity.teacher.mapper.getTeacher",id);
		session.close();
		return teacher;
	}
}

```

```java
package cn.sxt.test;

import java.io.IOException;
import java.util.List;
import cn.sxt.dao.StudentDao;
import cn.sxt.dao.TeacherDao;
import cn.sxt.entity.Student;
import cn.sxt.entity.Teacher;


public class Test {

	public static void main(String[] args) throws IOException {
		TeacherDao teacherDao =new TeacherDao();
		Teacher teacher = teacherDao.getTeacher(1);
		System.out.println(teacher.toString());
		List<Student>students = teacher.getStudents();
		for (Student student : students) {
			System.out.println(student.toString());
		}
		
	}

}

```

第二种方式：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.student.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
	<select id="getStudentById" resultType="Student">
		select * from student where tid=#{tid}
	</select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.teacher.mapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
	<!--
	<select id="getTeacher" resultMap="TeacherStudent">
		select s.id sid,s.name sname,s.tid stid,t.id tid ,t.name tanme  from student s,teacher t where s.tid=t.id and t.id=#{id}
	</select>
	<resultMap type="Teacher" id="TeacherStudent">
		<id column="tid" property="id"/>
		<result column="tname" property="name"/>
		<collection property="students" ofType="Student">
			<id column="sid" property="id"/>
			<result column="sname" property="name"/>
		</collection>
	</resultMap>
	注意：oftype -->
	<select id="getTeacher" resultMap="TeacherStudent">
		select * from teacher where id=#{id}
	</select>
	<resultMap type="Teacher" id="TeacherStudent">
		<collection property="students"  javaType ="ArrayList"  ofType="Student" column="id" select="cn.sxt.entity.student.mapper.getStudentById"></collection>
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



