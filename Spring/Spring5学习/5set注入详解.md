### 第五章，set注入详解

#### 1.详解主要是针对不同变量

~~~markdown
1.针对不同类型的成员变量，在<property>标签中，需要嵌套的类型标签包含哪些
	<property>
		<JDK包含的类型>
		<自定义类型>
	</property>
~~~

![image-20200729144219945](E:\MarkdownPicture\image-20200729144219945.png)

##### 1.1JDK内置类型

###### 1.1.1String+八种基本数据类型

~~~xml
	<property>
		<value>数据</value>
	</property>
~~~

###### 1.1.2数组类型的配置

~~~xml
<property name="bool">
	 <list>
           <value>leiyc5516@163.com</value>
           <value>leiyc@163.com</value>
           <value>1776109898@qq.com</value>
      </list>
</property>
~~~

###### 1.1.3set集合标签

~~~xml
<property name="famliy">
    <set>
         <value>baba</value>
         <value>mama</value>
         <value>didi</value>
        <ref bean=""></ref>
        <!--自定义类型的引用-->
    </set>
</property>
~~~

###### 1.1.4List集合标签

~~~xml
    <property name="tel">
            <list>
                <value>中关村</value>
                <value>中科院</value>
                <!--可以包含自定义类型引用以及各种集合数据-->
            </list>
    </property>
~~~

###### 1.1.5map集合对应的标签

~~~xml
        <property name="qqs">
            <map>
                <entry>
                    <key><value>1776109898</value></key>
                    <value>leiyc</value>
                </entry>
                <entry>
                    <key><value>342564</value></key>
                    <value>lihai</value>
                </entry>
            </map>
        </property>
~~~

###### 1.1.6 properties

特殊map   key---value 都必须为String

~~~xml
        <property name="p">
            <props>
                <prop key="key1">value1</prop>
                <prop key="key2">value2</prop>
            </props>
        </property>
~~~

###### 1.1.7复杂的数据类型类似于Date等数据后续需要自定义转换器实现

##### 1.2用户自定义类型

###### 1.2.1第一种方式

~~~markdown
1.必须实现为成员变量实现get/set方法
2.配置文件实现依赖注入,主要通过<bean>标签实现一个对象的产生
~~~



~~~xml
    <bean id="user" class="org.example.UserServiceImpl">
        <property name="userDAO">
            <bean class="org.example.UserDAOImpl"></bean>
        </property>
    </bean>
~~~



###### 1.2.2第二种方式

~~~markdown
1.第一种的配置文件代码冗余
2.<bean>标签多次创建对象
~~~

###### 步骤

~~~markdown
1.必须实现为成员变量实现get/set方法
2.配置文件实现依赖注入,主要通过<bean>标签实现一个对象的产生
~~~

~~~xml
    <bean id="userDAO" class="org.example.UserDAOImpl"></bean>
    <bean id="user" class="org.example.UserServiceImpl">
        <property name="userDAO">
           <ref bean="userDAO"></ref>
        </property>
    </bean>
~~~

#### 2.set注入的简化写法

##### 2.1基于属性的简化

~~~xml
JDK类型注入
<property name="name">
    <value>suns</value>
</property>
<property name="name" value="suns"></property>
注意： vlaue是属性 只能简化 8中基本数据类型和String

<property name="userDAO" ref="UserDAO"></property>
通过ref属性替换掉ref标签简化写法，替换掉的代码：
   <property name="userDAO">
           <ref bean="userDAO"></ref>
    </property>
	
~~~

##### 2.2基于命名空间p简化

~~~xml
    <bean id="homeaddressID" class="com.canyugan.setter.address">
		<property name="name" value="中关村"></property>
		<property name="tel" value="1816604"></property>
	</bean>
	<bean id="companyaddressID" class="com.canyugan.setter.address">
		<property name="name" value="北京天安门"></property>
		<property name="tel" value="123456"></property>
	</bean>
	<bean id="personID" class="com.canyugan.setter.person">
		<property name="name" value="参与感"/>
		<property name="homeAddress" ref="homeaddressID"/>
		<property name="companyAddress" ref="companyaddressID"/>
	</bean>
	简化的作为以下代码
	<!-- 
	p命名空间
	企业级开发用的比较少 
	-->
	<bean id="homeaddressID" class="com.canyugan.setter.address"
		  p:name="中环村"
		  p:tel="1816604">
	</bean>
	<bean id="companyaddressID" class="com.canyugan.setter.address"
		  p:name="北京天安门"
		  p:tel="123456">
	</bean>
	<bean id="personID" 
		  class="com.canyugan.setter.person" 
		  p:name="参与感"
		  p:homeAddress-ref="homeaddressID"
		  p:companyAddress-ref="companyaddressID">
    </bean>
~~~

