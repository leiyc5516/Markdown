>RDBMS
关系数据库管理系统（Relational Database Management System：RDBMS）是指包括相互联系的逻辑组织和存取这些数据的一套程序 (数据库管理系统软件)。关系数据库管理系统就是管理关系数据库，并将数据逻辑组织的系统。

>DataBase:就相当于一个仓库含有许多的数据记录表，表之间还含有关系

>Table :表结构，包含结构属性就是表的列名，和多条记录

>SQL语言学习（结构化查询语言），用于客服端与服务器对话使用

>DDL:【数据库模式定义语言]】DDL(Data Definition Language)，是用于描述数据库中要存储的现实世界实体的语言。用来定义数据库对象：例如库，表，列：
创建，删除，修改：库，表结构！！！（***量大****）

>DML:【数据库操作语言】：用来定义数据库记录>增，删，改，查（***重点***）

>DCL:【数据库控制语言】：用来定义数据库访问权限,创建新用户。

>DQL:【数据库查询语言】：用于查询数据（***难点***）

>MySQL建议关键字大写
显示所有数据库：
```
 SHOW DATABASES;
```
![image.png](https://upload-images.jianshu.io/upload_images/14935748-968b8c4a15d23d35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

切换数据库：
```
USE 数据库名称;
```
![image.png](https://upload-images.jianshu.io/upload_images/14935748-ebe2a2dc1471a83b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建数据库：
```
CREATE  DATDBASE 数据库名;
CREATE  DATDBASE 数据库名 utf8;//指定编码
```
删除数据库：
```
DROP DATABSE 数据库名称;
```
创建表:
```
CREATE TABLE 表名 （
    列名 列类型 ，
    列名 列类型 ，
    列名 列类型 ，
    列名 列类型 
）；
```
查看当前所有表：
```
SHOW TABLES ；
```
查询表结构：
```
DESC 表名字;
```
删除表：
```
DROP TABLE 表名称;
```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-ac42a920cef6366c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改表添加列：
```
ALTER TABLE 表名 ADD(
    列名 列类型 ，
    列名 列类型 ，
    列名 列类型 ，
    列名 列类型 
）；
```
修改表的列类型（如果修改的表的列存储了数据那么该数据类型会受到影响
```
ALTER TABLE 表名 NOTIFY 列名 列类型；
```

修改列名字：
```
ALTER TABLE 表名 原列名 新列名；
```
删除列：
```
ALTER TABLE 表名 DROP 列名；
```
修改表名称：
```
ALTER TABLE 原表名RENAME TO 新表名;
```

