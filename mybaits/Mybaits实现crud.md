> 利用Mybaits实现增删改查：
>
> >
> >
> >```xml 
> ><?xml version="1.0" encoding="UTF-8" ?>
> ><!DOCTYPE mapper
> > PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
> > "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
> ><mapper namespace="cn.sxt.entity.UserMapper"><!-- 设置mapper的名字 -->
> > 	<select id="selectUser" resultType="cn.sxt.entity.User"><!--id 作为select辨识符号 resultType="包名和类名" -->
> > 		select * from user where id = #{id}<!-- sql语句，#{注入的值} -->
> > 	</select>
> > 	<insert id="insertUser" parameterType="cn.sxt.entity.User" useGeneratedKeys="true">
> > 		insert into user(id,name,password) values(#{id},#{name},#{password})
> > 	</insert>
> > 	<update id="updateUser" parameterType="cn.sxt.entity.User" >
> > 		update user set name=#{name},password=#{password} where id=#{id}
> > 	</update>
> > 	<delete id="deleteUser">
> > 		delete from user where id=#{id}
> > 	</delete>
> ></mapper>
> >```
> >
> >```xml
> ><?xml version="1.0" encoding="UTF-8" ?>
> ><!DOCTYPE configuration
> > PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
> > "http://mybatis.org/dtd/mybatis-3-config.dtd">
> ><configuration>
> >	<environments default="development">
> >	 <environment id="development">
> >	 	<transactionManager type="JDBC"/>
> >		 <dataSource type="POOLED">
> >		 	<property name="driver" value="com.mysql.cj.jdbc.Driver"/><!-- 设置驱动的值 -->
> >			<property name="url" value="jdbc:mysql://127.0.0.1:3306/mydb?serverTimezone=UTC"/><!-- 设置数据库值 -->
> >			<property name="username" value="root"/><!-- 设置用户的值 -->
> >		 	<property name="password" value="qqzz1776109898"/><!-- 设置密码的值 -->
> >		 </dataSource>
> >	 </environment>
> >	</environments>
> >
> >	<mappers>
> >	 	<mapper resource="cn/sxt/entity/user.mapper.xml"/><!-- 导入mapper文件 -->
> >	</mappers>
> >
> ></configuration>
> >```
> >
> >```java
> >package cn.sxt.dao;
> >
> >import java.io.IOException;
> >
> >import org.apache.ibatis.session.SqlSession;
> >
> >import cn.sxt.entity.User;
> >import cn.sxt.utils.MybatisUtil;
> >
> >public class UserDao {
> >	public User getById(int id) throws IOException {
> >		SqlSession sqlSession = MybatisUtil.getSqlSession();
> >		User user = sqlSession.selectOne("cn.sxt.entity.UserMapper.selectUser", id);
> >		sqlSession.close();
> >		return user;
> >	}
> >	public int insertUser(User user) throws IOException {
> >		SqlSession sqlSession = MybatisUtil.getSqlSession();
> >		int result = sqlSession.insert("cn.sxt.entity.UserMapper.insertUser", user);
> >		sqlSession.commit();
> >		sqlSession.close();
> >		return result;
> >		
> >	}
> >	public int updateUser(User user) throws IOException {
> >		SqlSession sqlSession = MybatisUtil.getSqlSession();
> >		int result = sqlSession.update("cn.sxt.entity.UserMapper.updateUser", user);
> >		sqlSession.commit();
> >		sqlSession.close();
> >		return result;
> >		
> >	}
> >	public int deleteUser(int id) throws IOException {
> >		SqlSession sqlSession = MybatisUtil.getSqlSession();
> >		int result = sqlSession.delete("cn.sxt.entity.UserMapper.deleteUser", id);
> >		sqlSession.commit();
> >		sqlSession.close();
> >		return result;
> >	}
> >
> >}
> >
> >```
> >
> >```java
> >package cn.sxt.utils;
> >
> >import java.io.IOException;
> >import java.io.InputStream;
> >
> >import org.apache.ibatis.io.Resources;
> >import org.apache.ibatis.session.SqlSession;
> >import org.apache.ibatis.session.SqlSessionFactory;
> >import org.apache.ibatis.session.SqlSessionFactoryBuilder;
> >
> >public class MybatisUtil {
> >	//创建SqlSessionFactory
> >	public static SqlSessionFactory getSqlSessionFactory() throws IOException {
> >		String resource = "mybatis-config.xml";
> >		InputStream inputStream = Resources.getResourceAsStream(resource);
> >		SqlSessionFactory sqlSessionFactory =
> >		 new SqlSessionFactoryBuilder().build(inputStream);
> >		return sqlSessionFactory;
> >	}
> >	//创建SqlSession
> >	public static SqlSession getSqlSession() throws IOException {
> >		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
> >		return sqlSessionFactory.openSession();
> >		
> >	}
> >}
> >
> >```
> >
> >```java
> >package cn.sxt.entity;
> >//实体类的定义，实体bean
> >public class User {
> >	private int id;
> >	private String name;
> >	private String password;
> >	public int getId() {
> >		return id;
> >	}
> >	public void setId(int id) {
> >		this.id = id;
> >	}
> >	public String getName() {
> >		return name;
> >	}
> >	public void setName(String name) {
> >		this.name = name;
> >	}
> >	public String getPassword() {
> >		return password;
> >	}
> >	public void setPassword(String password) {
> >		this.password = password;
> >	}
> >	@Override
> >	public String toString() {
> >		return "User [id=" + id + ", name=" + name + ", password=" + password + "]";
> >	}
> >
> >
> >}
> >
> >```
> >
> >```java
> >package cn.sxt.test;
> >
> >import java.io.IOException;
> >
> >import org.apache.ibatis.session.SqlSession;
> >
> >import com.mysql.cj.Session;
> >
> >import cn.sxt.dao.UserDao;
> >import cn.sxt.entity.User;
> >import cn.sxt.utils.MybatisUtil;
> >
> >public class Test {
> >
> >	public static void main(String[] args) throws IOException {
> >
> >       // SqlSession sqlSession = MybatisUtil.getSqlSession();
> >   //     User user = sqlSession.selectOne("cn.sxt.entity.UserMapper.selectUser",6);
> >//		System.out.println(user.toString());
> >        	//	UserDao userDao = new UserDao();
> >//		User user = new User();
> >//		user.setId(7);
> >//		user.setName("小梦梦");
> >//		user.setPassword("897");
> >//		userDao.insertUser(user);
> >//		User user = userDao.getById(1);
> >//		userDao.updateUser(user);
> >//		System.out.println(userDao.updateUser(user));
> >//		System.out.println(userDao.deleteUser(1));
> >		
> >	}
> >	}
> >
> >
> >
> >}
> >
> >}
> >
> >```
> >
> >