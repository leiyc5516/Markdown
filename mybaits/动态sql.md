动态sql指的是根据不同的查询条件生成对应的查询语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper空间的名字防止SQL语句id重名 -->
 	<select id="getUserByCondition" parameterType="Map" resultType="User">
 		select * from user
 			<where><!-- Map传入参数，if做判断-->
	 			<if test="name!=null">
	 				name like "%"#{name}"%"
	 			</if>
                <if test="name!=null"><!-- 第二个if中sql语句要使用AND-->
	 				AND password like "%"#{password}"%"
	 			</if>
 			
 			</where>
 	</select>
</mapper>
```

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
		String resource = "mybatis-config2.xml";
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
package cn.sxt.entity;
//实体类的定义，实体bean
public class User {
	private int id;
	private String name;
	private String password;
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
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", password=" + password + "]";
	}


}

```

```java
package cn.sxt.dao;

import java.io.IOException;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.session.SqlSession;

import cn.sxt.entity.User;
import cn.sxt.utils.MybatisUtil;

public class UserDao {
	public List<User> getAll(Map<String, Object> map) throws IOException {
		SqlSession sqlSession = MybatisUtil.getSqlSession();
		List<User>  users= sqlSession.selectList("cn.sxt.entity.UserMapper.getUserByCondition", map);
		sqlSession.commit();
		sqlSession.close();
		return users;
	}

}

```

```java
package cn.sxt.test;

import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.session.SqlSession;

import com.mysql.cj.Session;

import cn.sxt.dao.UserDao;
import cn.sxt.entity.User;
import cn.sxt.utils.MybatisUtil;

public class Test {

	public static void main(String[] args) throws IOException {
		UserDao userDao =new UserDao();
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("name", "小");
		List<User> users = userDao.getAll(map);
		for (User user : users) {
			System.out.println(user.toString());
		}
	}
}

```

