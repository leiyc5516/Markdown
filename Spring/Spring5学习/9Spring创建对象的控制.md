### 第九章，控制Spring中的对象（Bean）创建次数

#### 1.控制简单对象只需要在配置文件指定scope

~~~xml
<bean id="account"  scope="singleton" class="org.example.scope.Account"></bean>
~~~

##### 一般情况scope 都是默认为singleton,如果需要创建多个这样的对象需要指定scope为proptotype

#### 2.如何控制复杂对象的创建次数？

~~~Java
public void FactoryBean()｛
    issingleton()｛
    return false;//创建多次
    return true;//创建一次
    ｝
 ｝
~~~

#### 3.如果不是使用实现FactoryBean接口类创建的对象，需要和简单对象指定scope一样

#### 4.为什么要控制对象创建次数

~~~markdown
	节省了内存空间不被浪费，一般都是可以共用的使用单例模式，不可以共用的最好使用多例模式
~~~

~~~markdown
创建一次的对象（可以共用）：
	SqlSessionFactory
	DAO
	Service
创建多次的对象（不可共用）：
	Connection
	SqlSession	
~~~

