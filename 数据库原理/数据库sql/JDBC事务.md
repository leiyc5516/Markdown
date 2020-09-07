>事务
**原子性**原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
**一致性**事务前后数据的完整性必须保持一致。
**隔离性**事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。
**持久性**持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响
举个简单的例子理解以上四点
####在MySQL中
**开启事务**     start transaction;
**结束事务**     commit;或者rollback;
  在执行SQL语句之前，先执行start transaction，这就开启了一个事务（事务的起点），然后可以去执行多条SQL语句，最后要结束事务，commit表示提交，即事务中的多条SQL语句所作出的影响会持久到数据库中，或者rollback，表示回滚到事务的起点，之前做的所有操作都被撤销了。
*********
#### 在JDBC中LConnection的三个方法与事务有关：
**setAutoCommit（boolean）**:设置是否为自动提交事务，如果true（默认值为true）表示自动提交，也就是每条执行的SQL语句都是一个单独的事务，如果设置为false，那么相当于开启了事务了；con.setAutoCommit(false) 表示开启事务。
**commit（）**：提交结束事务。
**rollback（）**：回滚结束事务。

******
#### JDBC中处理事务的一般格式
```
try{
     con.setAutoCommit(false);//开启事务
     ......
     con.commit();//try的最后提交事务      
} catch（） {
    con.rollback();//回滚事务
}
```

```
package andvanceJDBC;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class AC {
    /*
    * 修改指定用户的余额
    * */
    public void updateBalance(Connection con, String name,double balance) {
        try {
            String sql = "UPDATE account SET accountNum =accountNum + ? WHERE accountName= ?";
            PreparedStatement pstmt = con.prepareStatement(sql);
            pstmt.setDouble(1,balance);
            pstmt.setString(2,name);
            pstmt.executeUpdate();
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
```
package andvanceJDBC;

import org.junit.Test;
import java.sql.Connection;
import java.sql.SQLException;

public class FTO {
    /*
    * 演示转账方法
    * 所有对Connect的操作都在Service层进行的处理
    * 把所有connection的操作隐藏起来，这需要使用自定义的小工具（day19_1）
    * */
  
	public void transferAccounts(String from,String to,double money) {
        //对事务的操作
        Connection con = null;
        try{
            con = JDBCUntil.getConnection();
            con.setAutoCommit(false);
            AC dao = new AC();
            dao.updateBalance(con,from,-money);//给from减去相应金额
            /*
             * if (true){
             *
             *    throw new RuntimeException("不好意思，转账失败");
             *}
             */
            dao.updateBalance(con,to,+money);//给to加上相应金额
            //提交事务
            con.commit();

        } catch (Exception e) {
            try {
                con.rollback();
            } catch (SQLException e1) {
                e.printStackTrace();
            }
            throw new RuntimeException(e);
        }
    }
    @Test
    public void fun1() {
        transferAccounts("zs","ls",100);
    }
}
```
#### 事务隔离级别

>1、事务的并发读问题
>>脏读：读取到另外一个事务未提交数据（不允许出来的事）；
>>不可重复读：两次读取不一致；
>>幻读（虚读）：读到另一事务已提交数据。

>2、并发事务问题
>>因为并发事务导致的问题大致有5类，其中两类是更新问题三类是读问题。
>>脏读（dirty read）：读到另一个事务的未提交新数据，即读取到了脏数据；
>>不可重复读（unrepeatable）：对同一记录的两次读取不一致，因为另一事务对该记录做了修改；
>>幻读（虚读）（phantom read）：对同一张表的两次查询不一致，因为另一事务插入了一条记录。

>3、四大隔离级别
**4个等级的事务隔离级别，在相同的数据环境下，使用相同的输入，执行相同的工作，根据不同的隔离级别，可以导致不同的结果。不同事务隔离级别能够解决的数据并发问题的能力是不同的。**
>>1、SERIALIZABLE(串行化)
不会出现任何并发问题，因为它是对同一数据的访问是串行的，非并发访问的；
性能最差
2、REPEATABLE READ(可重复读)（MySQL）
防止脏读和不可重复读，不能处理幻读
性能比SERIALIZABLE好
3、READ COMMITTED(读已提交数据)（Oracle）
防止脏读，不能处理不可重复读和幻读；
性能比REPEATABLE READ好
4、READ UNCOMMITTED(读未提交数据)
可能出现任何事物并发问题，什么都不处理。
性能最好

###  MySQL隔离级别

**MySQL的默认隔离级别为Repeatable read,可以通过下面语句查看：
SELECT @@`TX_ISOLATION`;
也可以通过下面语句来设置当前连接的隔离级别：
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ ;//[4选1]**
### JDBC设置隔离级别
**con.setTransactionIsolation(int level) :参数可选值如下：
Connection.TRANSACTION_READ_UNCOMMITTED；
Connection.TRANSACTION_READ_COMMITTED；
Connection.TRANSACTION_REPEATABLE_READ；
Connection.TRANSACTION_READ_SERIALIZABLE。**