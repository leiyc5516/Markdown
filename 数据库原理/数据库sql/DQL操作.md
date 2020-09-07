>基本查询
```
SELECT  *  FROM   表名；//查询所有列
````
>指定列查询
```
SELECT  列名1，列名2，列名3....... FROM 表名；//查询指定表名得指定列得数据
```
>完全重复的记录只有一条
```
SELECT  DISTINCT  *  列名1，列名2，列名3....... FROM 表名；//查询记录出现相同记录显示一条记录
```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-9c6ab4343f7961ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-faab5ba8eaaf0124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>升序降序排列
```
--AddTime 升序,ID 升序
select * from DS_Finance ORDER BY AddTime,ID; 
 
--AddTime 升序,ID降序
select * from DS_Finance ORDER BY AddTime,ID DESC;
 
--AddTime 降序,ID升序
select * from DS_Finance ORDER BY AddTime DESC,ID ;
 
--AddTime 降序,ID降序
select * from DS_Finance ORDER BY AddTime DESC,ID DESC; 
```

聚合函数是对一组值执行计算并返回单一的值的函数，它经常与SELECT语句的GROUP BY子句一同使用，SQL SERVER 中具体有哪些聚合函数呢？我们来一一看一下：
1. AVG  返回指定组中的平均值，空值被忽略。
    例：select  prd_no,avg(qty) from sales group by prd_no

2. COUNT  返回指定组中项目的数量。
      例：select  count(prd_no) from sales 

3. MAX  返回指定数据的最大值。
      例：select  prd_no,max(qty) from sales group by prd_no 

4. MIN  返回指定数据的最小值。
      例：select  prd_no,min(qty) from sales group by prd_no

5. SUM  返回指定数据的和，只能用于数字列，空值被忽略。
      例：select  prd_no,sum(qty) from sales group by prd_no

6. COUNT_BIG  返回指定组中的项目数量，与COUNT函数不同的是COUNT_BIG返回bigint值，而COUNT返回的是int值。
     例：select  count_big(prd_no) from sales

7. GROUPING  产生一个附加的列，当用CUBE或ROLLUP运算符添加行时，输出值为1.当所添加的行不是由CUBE或ROLLUP产生时，输出值为0.
     例：select  prd_no,sum(qty),grouping(prd_no) from sales group by prd_no with rollup

8. BINARY_CHECKSUM  返回对表中的行或表达式列表计算的二进制校验值，用于检测表中行的更改。
     例：select  prd_no,binary_checksum(qty) from sales group by prd_no

9. CHECKSUM_AGG  返回指定数据的校验值，空值被忽略。
     例：select  prd_no,checksum_agg(binary_checksum(*)) from sales group by prd_no

10. CHECKSUM  返回在表的行上或在表达式列表上计算的校验值，用于生成哈希索引。

11. STDEV  返回给定表达式中所有值的统计标准偏差。
     例：select  stdev(prd_no) from sales

12. STDEVP  返回给定表达式中的所有值的填充统计标准偏差。
     例：select  stdevp(prd_no) from sales

13. VAR  返回给定表达式中所有值的统计方差。
     例：select  var(prd_no) from sales

14. VARP  返回给定表达式中所有值的填充的统计方差。
     例：select  varp(prd_no) from sales


# [摘抄于该网站](https://www.cnblogs.com/friday69/p/9389720.html)

employee 表

| id | name | gender | hire_date | salary | performance | manage | deparmant |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1001 | 张三 | 男 | 2/12/1991 00:00:00 | 2000 | 200 | 500 | 营销部 |
| 1002 | 李四 | 男 | 5/8/1993 00:00:00 | 4000 | 500 |  | 营销部 |
| 1003 | 王五 | 女 | 12/13/1993 00:00:00 | 1000 | 100 | 5000 | 研发部 |
| 1004 | 赵六 | 男 | 8/19/1996 00:00:00 | 8000 | 1000 | 4000 | 财务部 |
| 1005 | 孙七 | 女 | 11/6/1997 00:00:00 | 5000 | 500 |  | 研发部 |
| 1006 | 周八 | 男 | 10/16/1994 00:00:00 | 6000 | 2000 | 1000 | 人事部 |
| 1007 | 吴九 | 女 | 9/22/1995 00:00:00 | 8000 | 1500 |  | 研发部 |
| 1008 | 郑十 | 女 | 10/25/1998 00:00:00 | 4000 | 900 |  | 人事部 |

## 1.SQL分组查询GroupBy+Group_concat

group by 是分组，是分组，是分组，分组并不是去重，而是分组

将查询结果按一个或多个进行分组，字段值相同的为一组

GroupBy+Group_concat ： 表示分组之后，根据分组结果，使用 group_contact() 来放置每一组的每字段的值的集合

```
select deparmant, GROUP_CONCAT(`name`) from employee GROUP BY deparmant
```

![image](https://upload-images.jianshu.io/upload_images/14935748-b66c831821ad13bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据 department 分组，通过 group_concat（'name'）,查看每组里面的姓名都有哪些

​

```
SELECT gender,GROUP_CONCAT(`name`) from employee GROUP BY gender
```

![image](https://upload-images.jianshu.io/upload_images/14935748-dd296c59b74a261d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据gender 分类，看 不同的 性别都有哪些 人

分组注意事项： 在分组时，select后面跟的的字段一般都会出现在 group by 后

```
SELECT name,gender from employee GROUP BY gender,name
-- 先按gender分组，再按姓名分组...
```

![image](https://upload-images.jianshu.io/upload_images/14935748-b8d2fce56fb109cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

* * *

​

​

## 2.SQL分组+聚合函数

```
select deparmant, GROUP_CONCAT(salary), SUM(salary),AVG(salary) 平均工资,MAX(salary) 最高工资 from employee GROUP BY deparmant;
-- 根据department 分组，计算各部门下工资总数，平均工资，最高工资![1532919789347](D:\Python\python_learning\Python_Blog\02\SQL\4.png)
```

![image](https://upload-images.jianshu.io/upload_images/14935748-4158f73a899a968a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

​

```
-- 查询每个部门的部门名称以及每个部门的人数
SELECT deparmant, GROUP_CONCAT(`name`), COUNT(*) from employee GROUP BY deparmant
```

![image](https://upload-images.jianshu.io/upload_images/14935748-21357a2ce630b294.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

​

```
-- 查询每个部门的部门名称以及每个部门工资大于1500的人数
SELECT deparmant,GROUP_CONCAT(salary), COUNT(*) from employee WHERE salary > 1500 GROUP BY deparmant

```

![image](https://upload-images.jianshu.io/upload_images/14935748-3ca203a576a5dd5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

## 3.SQL分组GroupBy+Having

*   group by + having 用来分组查询后指定一些条件来输出查询结果
*   having 和 where 一样，但 having 只能用于 group by

```
-- 查询工资总和大于 9000的部门名称
SELECT deparmant, GROUP_CONCAT(salary), SUM(salary) FROM employee 
GROUP BY deparmant 
HAVING SUM(salary) > 9000;
```

![image](https://upload-images.jianshu.io/upload_images/14935748-50272bdade31ddb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

**having 和 where 的区别：**

*   having 是在分组后对数据进行过滤，where 是在分组前对数据进行过滤
*   having后面可以使用分组函数(统计函数)，where后面不可以使用分组函数
*   where 是对分组前记录的条件，如果某行记录没有满足where字句的条件，那么这行记录不会参加分组；而having是对分组后数据的约束

​

​

```
-- 查询工资大于2000的，工资总和大于9000的部门名称以及工资和
select deparmant,GROUP_CONCAT(salary), SUM(salary) from employee 
WHERE salary > 2000 
GROUP BY deparmant 
HAVING sum(salary) > 9000
ORDER BY SUM(salary) DESC;
```

![image](https://upload-images.jianshu.io/upload_images/14935748-533360c770c7d76f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​

## 4.sql语句书写顺序

![image](https://upload-images.jianshu.io/upload_images/14935748-15b8ff4065af4840.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


