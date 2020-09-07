>【JDBC连接数据库7个步骤】

>1.JDBC所需的四个参数（user，password，url，driverClass）
（1）user用户名
（2）password密码
（3）URL定义了连接数据库时的协议、子协议、数据源标识。    
    •书写形式：协议：子协议：数据源标识    
    协议：在JDBC中总是以jdbc开始    
    子协议：是桥连接的驱动程序或是数据库管理系统名称。    
    数据源标识：标记找到数据库来源的地址与连接端口。
(4）driverClass连接数据库所需的驱动。

>2.加载JDBC驱动程序：    
    在连接数据库之前，首先要加载想要连接的数据库的驱动到JVM（Java虚拟机），    
    这通过java.lang.Class类的静态方法forName(String  className)实现。

>3.创建数据库的连接    
    •要连接数据库，需要向java.sql.DriverManager请求并获得Connection对象，    
     该对象就代表一个数据库的连接。    
    •使用DriverManager的getConnectin(String url , String username ,     
    String password )方法传入指定的欲连接的数据库的路径、数据库的用户名和    
     密码来获得。    

>4.创建一个preparedStatement    
    •要执行SQL语句，必须获得java.sql.Statement实例，Statement实例分为以下3   
     种类型：    
      1、执行静态SQL语句。通常通过Statement实例实现。    
      2、执行动态SQL语句。通常通过PreparedStatement实例实现。    
      3、执行数据库存储过程。通常通过CallableStatement实例实现。

>5.执行SQL语句    
    Statement接口提供了三种执行SQL语句的方法：executeQuery 、executeUpdate    
   和execute    
    1、ResultSet executeQuery(String sqlString)：执行查询数据库的SQL语句    
        ，返回一个结果集（ResultSet）对象。    
     2、int executeUpdate(String sqlString)：用于执行INSERT、UPDATE或    
        DELETE语句以及SQL DDL语句，如：CREATE TABLE和DROP TABLE等    
     3、execute(sqlString):用于执行返回多个结果集、多个更新计数或二者组合的    
        语句。    

>6、遍历结果集    
    两种情况：    
     1、执行更新返回的是本次操作影响到的记录数。    
     2、执行查询返回的结果是一个ResultSet对象。    
    • ResultSet包含符合SQL语句中条件的所有行，并且它通过一套get方法提供了对这些    
      行中数据的访问。    
    • 使用结果集（ResultSet）对象的访问方法获取数据： 

>7、处理异常，关闭JDBC对象资源     
     操作完成以后要把所有使用的JDBC对象全都关闭，以释放JDBC资源，关闭顺序和声    
     明顺序相反：    
     1、先关闭requestSet    
     2、再关闭preparedStatement    
     3、最后关闭连接对象connection

#####实例演示：
```
		//连接数据库
		String url = "jdbc:mysql://127.0.0.1:3306/mydb?user=root&password=qqzz1776109898&useUnicode=true&characterEncoding=GB2312&useSSL=false&serverTimezone=UTC";
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con = DriverManager.getConnection(url);
```
```
Statement stmt = con.createStatement();//创建Statement
```

>statement是Java语言实现和数据库交互的接口
execute（sql）
执行指定的SQL语句，可能返回多个结果，如createe table或create db等指令。
excuteQuery(String sql)
执行给定的SQL语句，该语句返回单个ResultSet对象。
executeupdate(String sql)
执行SQL语句可能为insert、update、delete等不返回任何内容。主要用于DML与DDL
close（）关闭该对象。
数据添加、修改、删除均使用executeupdate函数，查询使用excuteQuery函数

####插入数据记录
```
	public static void insert(Statement stmt) throws SQLException {
		String insert = "INSERT INTO user VALUES ('4','小笨蛋','25','女')";
		stmt.executeUpdate(insert);
	}
```
####修改记录
```
public static void set(Statement stmt) throws SQLException {
		String set = "UPDATE user SET id='6',name='小可爱',age='25',sex='女' WHERE (id='4')";
		stmt.executeUpdate(set);
	}		
```
####删除数据
```
	public static void delete(Statement stmt) throws SQLException {
		String delete = "DELETE FROM user WHERE (id='5')";
		stmt.executeUpdate(delete);
		
	}
```
####查询数据
![image.png](https://upload-images.jianshu.io/upload_images/14935748-a69aea9b59653621.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
	public static void query(Statement stmt ) throws SQLException {
		String query = "SELECT * FROM user";
		ResultSet set = stmt.executeQuery(query);
		while (set.next()) {
			/*
			 * 通过列编号获取数值
			 */
			int  id  = set.getInt(1);
			String name = set.getString(2);
			int age = set.getInt(3);
			String sex = set.getString(4);
			System.out.println(id+name+sex+age);
		}		
	}
```
![image.png](https://upload-images.jianshu.io/upload_images/14935748-ca9a54316cc621bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-89697f2e9390b125.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
/*
*关闭资源
*/
set.close();
stmt.close();
con.close();
```
####完整代码，主函数部分注释掉了
```
package com.learn;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestJDBC {

	public static void main(String[] args) throws Exception {
		//连接数据库
		String url = "jdbc:mysql://127.0.0.1:3306/mydb?user=root&password=qqzz1776109898&useUnicode=true&characterEncoding=GB2312&useSSL=false&serverTimezone=UTC";
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection con = DriverManager.getConnection(url);
		/*
		 * 对数据库增删改查
		 * 利用Connection实现statement对象的创建
		 * 利用statement实现与数据库语句的交互
		 */
		/*
		 * 修改记录
		 */
		Statement stmt = con.createStatement();//创建这样的一个可以用来交互的接口，发送数据库语言
		//insert(stmt);	
		//set(stmt);		
		//delete(stmt);
		query(stmt);
		stmt.close();
		con.close();
		

	}
	/*
	 * 更新设置数据
	 */

	public static void set(Statement stmt) throws SQLException {
		String set = "UPDATE user SET id='6',name='小可爱',age='25',sex='女' WHERE (id='4')";
		stmt.executeUpdate(set);
	}
	/*
	 * 插入数据
	 */
	public static void insert(Statement stmt) throws SQLException {
		String insert = "INSERT INTO user VALUES ('4','小笨蛋','25','女')";
		stmt.executeUpdate(insert);
	}
	/*
	 * 删除数据记录
	 * 
	 */
	public static void delete(Statement stmt) throws SQLException {
		String delete = "DELETE FROM user WHERE (id='5')";
		stmt.executeUpdate(delete);
		
	}
	public static void query(Statement stmt ) throws SQLException {
		String query = "SELECT * FROM user";
		ResultSet set = stmt.executeQuery(query);
		while (set.next()) {
			/*
			 * 通过列编号获取数值
			 */
			int  id  = set.getInt(1);
			String name = set.getString(2);
			int age = set.getInt(3);
			String sex = set.getString(4);
			System.out.println(id+name+sex+age);
		}
		set.close();
	}

}
```

####QUERY标准化操作
```
public static void Basic() throws SQLException {
		Connection con =null;
		Statement stmt =null;
		ResultSet set = null;
		String url = "jdbc:mysql://127.0.0.1:3306/mydb?user=root&password=qqzz1776109898&useUnicode=true&characterEncoding=GB2312&useSSL=false&serverTimezone=UTC";
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			 con = DriverManager.getConnection(url);
			 stmt = con.createStatement();
			 String query = "SELECT * FROM user";
              /*
               *执行query语句
              */
			 set = stmt.executeQuery(query);
			while (set.next()) {
					/*
					 * 通过列编号获取数值
					 */
				int  id  = set.getInt(1);
				String name = set.getString(2);
				int age = set.getInt(3);
				String sex = set.getString(4);
				System.out.println(id+name+sex+age);
			}
				
		} catch (Exception e) {
			e.printStackTrace();
		}
		finally {//关闭流
			if (set!= null ) set.close();
			if (stmt!= null )stmt.close();
			if (con!= null )con.close();		
		}
		
		
	}
```

>一、移动游标：
    1、void beforeFirst()：把光标放到第一行的前面，这也是光标默认的位置（虚拟位置）；
    2、void afterLast()：把光标放到最后一行的后面（虚拟位置）；
    3、boolean first()：把光标放到第一行的位置，返回值表示调控光标是否成功；
    4、boolean previous():把光标向上挪一行；
    5、boolean last():把光标放到最后一行的位置上；
    6、boolean next()：把光标向下挪一行；
    7、boolean relative(int row):相对位移，当row为正数时，表示向下移动row行，为负数时表示向上移动row行；
    8、boolean absolute(int row):绝对位移，把光标移动到指定的行上；

>二、判断游标：
    1、boolean isBeforeFirst():当前光标位置是否在第一行的前面；
    2、boolean isAfterLast():当前光标位置是否在最后一行的后面；
    3、boolean isFirst():当前光标位置是否在第一行上；
    4、boolean isLast():当前光标位置是否在最后一行上；
    5、int getRow()：返回当前光标的位置；

>三、获取总行数：先执行rs.last()把光标移动到最后一行，再执行rs.getRow()；获得当前光标所在行，可以得到结果集一共有多少行；

>四、获取总列数：
    1、先获取结果集的元数据ResultSetMetaData ramd=ra.getMetaData();
    2、获取结果集列数：int len=ramd.getColumnCount();
    3、获取指定列的列名：String name=ramd.getColumnName(int colIndex);

二、批量更新

1 Statement批量更新

语法:
```
Statement stm = con.createStatement();
stm.addBatch(sqlString); stm.addBatch(sqlString);……
stm.executeBatch();
```
```
演示样例:
con.setAutoCommit(false);       //设置事务非自己主动提交
Statement stm = con.createStatement();
stm.addBatch(“insert into t_user(id, name, password) values(11, ‘Rose’, ‘Rose’)”);
stm.addBatch (“insert into t_user(id, name, password) values(12, ‘Mary’, ‘Mary’)”);
int[] results = stm.executeBatch();      //提交运行
con.commit();                //提交事务。使更改成为持久更改
```


批量更新一定要将事务提交设为非自己主动提交
```
con.setAutoCommit(false);
con.commit();
```
Statement的executeBatch()方法提交多个命令到数据库运行。返回每一个命令更新行数所组成的数组
假设批量更新中的某些命令无法正确运行，则会抛出 BatchUpdateException异常
BatchUpdateException的getUpdateCounts()方法能够返回发生此异常之前批量更新中成功运行的每一个更新语句的更新计数组成的数组
*****
####结果集的特性
是否可以滚动
是否敏感
是否可更新
con,createStatement():生成结果集：不滚动，不敏感，不可更行。
![image.png](https://upload-images.jianshu.io/upload_images/14935748-3625bdeaf6fe666d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



####PreparedStatement的用法

>相对Statement
>>1.可以防止SQL攻击
2.提高代码可读性
3.提高效率
```
	public static boolean loginL(String username,String password) throws Exception {
		String url = "jdbc:mysql://127.0.0.1:3306/mydb?user=root&password=qqzz1776109898&useUnicode=true&characterEncoding=GB2312&useSSL=false&serverTimezone=UTC";
		Connection con;
		PreparedStatement prestmt;
		
			Class.forName("com.mysql.cj.jdbc.Driver");
			con = DriverManager.getConnection(url);
			/*
			 * 创建sql模板，使用?代替参数
			 * 产生prepareStatement对象
			 * 调用prepareStatement的一系列setXxx()方法为？赋值
			 */
			String sql = "SELECT * FROM login WHERE username=? and password=?";
			prestmt = con.prepareStatement(sql);
			prestmt.setString(1, username);
			prestmt.setString(2, password);
			ResultSet set = prestmt.executeQuery();
			return set.next();
	}
```
********
####通过配置文件来配置加载数据库
**配置文件**
```
driverName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/mydb?useUnicode=true&characterEncoding=GB2312&useSSL=false&serverTimezone=UTC
user=root
password=qqzz1776109898
```
**加载配置文件实现读取配置文件得到相关信息**
```
package com.learn;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.util.Properties;

public class JDBCUntil {

	public static Connection getConnection() throws  Exception {
		/*
		 * 加载配置文件
		 * 获得的四大参数
		 * 加载驱动类
		 * 调用DriverManage。getConnection();获取连接
		 */
		/*
		 * 使用流打开配置文件,并加载文件
		 */
		InputStream in = JDBCUntil.class.getClassLoader().getResourceAsStream("db.properties");
		Properties pr= new Properties();
		pr.load(in);
		/*
		 * 加载驱动类
		 */
		Class.forName(pr.getProperty("driverName"));
		return DriverManager.getConnection(pr.getProperty("url"),pr.getProperty("user"),pr.getProperty("password"));
		
		
	}

}
```
**静态块优化配置文件多次加载的问题**
```
package com.learn;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.util.Properties;

public class JDBCUntil {
	static private Properties pr;
	static {
		/*
		 * 加载配置文件
		 * 获得的四大参数
		 * 加载驱动类
		 * 调用DriverManage。getConnection();获取连接
		 */
		/*
		 * 使用流打开配置文件,并加载文件
		 */
		InputStream in = JDBCUntil.class.getClassLoader().getResourceAsStream("db.properties");
		pr= new Properties();
		try {
			pr.load(in);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		/*
		 * 加载驱动类
		 */
		try {
			Class.forName(pr.getProperty("driverName"));
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

	public static Connection getConnection() throws  Exception {
		
		return DriverManager.getConnection(pr.getProperty("url"),pr.getProperty("user"),pr.getProperty("password"));
		
		
	}

}
```
