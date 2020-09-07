### 第二十四章，Spring的事务属性

#### 1.什么是事务属性

~~~markdown
* 1.属性：描述物体特征的一系列值 
* 事务属性：描述事务的一系列值
	* 隔离属性
	* 传播属性
	* 只读属性
	* 超时属性
	* 异常属性
~~~

#### 2.如何添加事务属性

~~~markdown
*	@Transactional(isloation="",popagation="",readOnly="",timeout="",rollbackFor="",noRollbackFor="")
* 与标题1上面的事务属性一一对相应
~~~

#### 3.事务属性详解

##### 3.1隔离属性

* 隔离属性的概念

~~~markdown
* 概念：事务描述事务解决并发的特征
	* 并发：同一时间段，访问相同数据
	* 并发产生哪些问题
		* 脏读
		* 不可重复度
		* 幻读
	* 并发问题解决：
		* 需要隔离属性解决
		* 设置隔离属性的不同值来实现并发问题
		
~~~

~~~markdown
* 脏读：一个事务读取另外一个事务未提交的数据，会在本事务中产生数据不一致的问题
	* 	解决方案 @Transactional(isloation=Isloation.READ_COMMITTED)
* 不可重复读
	* 一个事物多次读取相同数据，但是读取结果不一样，会在事务中导致数据不一致的问题
	* 注意：1.不是脏读 2.一个事务中
	* 	解决方案 @Transactional(isloation=Isloation.REPETEABLE_READ)
	* 本质是一把行锁

* 幻读
	* 一个事务中，多次对整个表查询统计，但是结果不一样，会在本事务中产生数据不一致的问题
	* 解决方案@Transactional(isloation=Isloation.SERIALIZBLE)
	* 本质：表锁
~~~



#### 总结

~~~markdown
* 并发安全：
 * @Transactional(isloation=Isloation.SERIALIZBLE)
 * @Transactional(isloation=Isloation.REPETEABLE_READ)
 * @Transactional(isloation=Isloation.READ_COMMITTED)
* 并发效率是反向的

~~~

#### 数据库对隔离属性的支持

| 隔离属性                  | MySQL | Oracle |
| ------------------------- | ----- | ------ |
| Isloation.READ_COMMITTED  | 支持  | 支持   |
| Isloation.REPETEABLE_READ | 支持  | 不支持 |
| Isloation.SERIALIZBLE     | 支持  | 支持   |

#### 默认隔离属性ISLOATION_DEFAULT,就是默认隔离属性



建议：我们在实战中我们需要应用默认值，在需要的情况我们采取修改，默认使用时都是使用的数据库的隔离属性

一般情况我们推荐使用乐观锁去实现隔离



------

#### 3.2传播属性

Service中嵌套Service的时候出现事务嵌套

~~~java
	Aservice{
        a(){
            B.b()
        }
    }

	Bservice{
        b(){
            
        }
    }
//上述的Asevice嵌套了B服务的方法，有事务的嵌套
~~~

#### 大事务中融入很多小事务，他们彼此影响，最终会导致部分外部大事务丧失原子性





#### 传播属性的值：

传播属性定义的是**当一个事务方法碰到另一个事务方法时的处理行为**，一共有七种行为，定义如下

| 传播性                      | 值   | 描述                                                         |
| --------------------------- | ---- | ------------------------------------------------------------ |
| `PROPAGATION_REQUIRED`      | 0    | 支持当前事务，如果没有就新建事务                             |
| `PROPAGATION_SUPPORTS`      | 1    | 支持当前事务，如果没有就不以事务的方式运行                   |
| `PROPAGATION_MANDATORY`     | 2    | 支持当前事务，如果当前没事务就抛异常                         |
| `PROPAGATION_REQUIRES_NEW`  | 3    | 无论当前是否有事务，都会新起一个事务                         |
| `PROPAGATION_NOT_SUPPORTED` | 4    | 不支持事务，如果当前存在事务，就将此事务挂起不以事务方式运行 |
| `PROPAGATION_NEVER`         | 5    | 不支持事务，如果有事务就抛异常                               |
| `PROPAGATION_NESTED`        | 6    | 如果当前存在事务，在当前事务中再新起一个事务                 |

#### 默认传播属性

`PROPAGATION_REQUIRED`是传播属性的默认值

#### 推荐使用的传播属性使用方式

~~~markdown
* 增删改：`PROPAGATION_REQUIRED`
* 查 ：`PROPAGATION_SUPPORTS`
~~~

#### 3.3只读属性readOnly默认false

针对只查询的业务方法，可以加入只读属性，提供运行效率

#### 3.4.超时属性（timeout）

指定事务等待的最长时间

​	当前事务访问数据，有可能 访问的数据 被别的事务加锁了，那么这个时候该等待

​	等待时间   秒

​	如何运用@Transactional（time= ?）默认值-1

#### 3.5.异常属性

Spring事务处理个过程中

默认 对RuntimeException及其子类 采用的是回滚的策略

默认 对Exception及其子类 采用的提交的策略



rollbackFor = (java.lang.Exception.xxx.xxx)

noRollbackFor =(java.lang.RuntimeException.xxx.xxx)

------

总结：

~~~markdown
* 隔离属性
* 传播属性
* 只读属性
* 超时属性
* 异常属性 	
~~~



