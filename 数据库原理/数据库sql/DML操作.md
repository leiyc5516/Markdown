>查询表记录 
```
SELECT * FROM 表名称;
```
>插入语句
```
INSERT INTO 表名 （列名1，列名2，列名3，列名4..........） VALUES (列值1，列值2，列值3，列值4.........);//列值全加上单引号
INSERT INTO `mydb`.`user` (`id`, `name`, `age`, `SEX`) VALUES ('4', '小海', '19', '男');
//对于部分列的插入，未插入值得列我们默认为默认值
```
>更新某行某列值
```
UPDATE 表名 SET 列名 = '列值’, 列名 = '列值’,列名 = '列值’,列名 = '列值’  WHERE (条件);//WHERE条件非必选项
UPDATE `mydb`.`user` SET `SEX` = '女' WHERE (`id` = '3');
```
>删除数据
```
DELETE  FROM  表名  WHERE (条件);
TRUNCATE  TABLE  表名 ；//TRUNCATE是DDL语句，他是先drop掉该表再create该表。而且无法回滚！！！！
```
