### 第十一章，配置文件参数化

~~~ markdown
什么是配置文件参数化
	就是把经常修改字符串的文件，转移到一个小的配置文件，简单来说就是把每个不同功能的项转移到各自单独的文件
~~~

~~~markdown
Spring中配置文件是否存在经常修改的字符串？
	存在：例如数据库连接参数 代表
经常变化的字符串，在Spring字符串中，直接修改	
	不利于项目维护（修改）
转到小的配置文件（.properties）
	利于维护（修改）
~~~

![image-20200810083540242](E:\Markdown\Spring\Spring5学习\image-20200810083540242.png)

主要就是将配置文件的经常修改的字符串转移到.properties中，通过${名字}获取值，这样修改的时候不需要去修改所谓的xml文件

~~~markdown
利于xml文件参数的修改，不用去xml找字符串位置
~~~

~~~properties
jdbc.driverClassName = com.mysql.cj.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/studydata?serverTimezone=UTC
jdbc.account = root
jdbc.password = qqzz1776109898
~~~

上面就是转移后的信息

然后在xml中通知这个小配置文件的位置，让xml文件去读取相关信息就好

~~~xml
<context:property-placeholder location="classpath:/sql.properties"></context:property-placeholder>
    <bean id="conn" class="org.example.sql.MysqlConnection">
            <property name="driverClassName" value="${jdbc.driverClassName}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="account" value="${jdbc.account}"></property>
            <property name="password" value="${jdbc.password}"></property>
        </bean>

~~~

~~~java
复杂的对象创建
 package org.example.sql;

import org.springframework.beans.factory.FactoryBean;

import java.sql.Connection;
import java.sql.DriverManager;

/**
 * @Author: Colin
 * @Date: 2020/8/10 9:33
 * @Version: 1.0
 * @Description:
 */
public class MysqlConnection implements FactoryBean<Connection> {
    private String driverClassName;
    private String url;
    private String account;
    private String password;

    public String getDriverClassName() {
        return driverClassName;
    }

    public void setDriverClassName(String driverClassName) {
        this.driverClassName = driverClassName;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "MysqlConnection{" +
                "driverClassName='" + driverClassName + '\'' +
                ", url='" + url + '\'' +
                ", account='" + account + '\'' +
                ", password='" + password + '\'' +
                '}';
    }

    @Override
    public Connection getObject() throws Exception {
        Class.forName(driverClassName);
        Connection con = DriverManager.getConnection(url,account,password );
        return con;
    }

    @Override
    public Class<?> getObjectType() {
        return Connection.class;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}

~~~

