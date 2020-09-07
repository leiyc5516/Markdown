##### 1.URL对应Bean

```xml
  <!-- 配置HandlerMapping -->
 <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
 </bean>
 <!-- 配置handlerAdapter -->
 <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter">
 </bean>
 <!-- 配置请求和处理器 -->
  <bean name="/hello.do" class="cn.sxt.hello.HelloController"></bean>
```

详见：Spring的第一个程序案例

这种方式一旦访问：/hello.do就会自动查询name为/hello.do的Bean并找到相对应的控制器

##### 2.URL分配Bean

```xml
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
  	<property name="mappings">
  		<props>
  			<!--key 对应URL请求名    value 对应处理器的ID  -->
  			<prop key="hello.path">HelloController</prop>
  		</props>
  	</property>
  </bean>
  <bean id="HellController" class="cn.sxt.controller.HelloControll"></bean>
```

详见SpringMVC代码/03springmvc_controller代码

此类配置还可以使用统配符号访问：/hello.do就会自动查询prop值（尖括号包裹的）对应该值为ID的Bean并找到相对应的控制器

##### 3.URL配置Bean<不要使用>

```xml 
<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping"></bean>
  	<!-- 请求Hello.*path都将被匹配 -->
  	<bean id="HelloController" class="cn.sxt.controller.HelloControll"></bean>
```

##### 4.注解实现配置

```xml 
 <!-- 配置注解扫描 -->
  <context:component-scan base-package="cn.sxt.controller"></context:component-scan>
```

将需要的Controller放在该包下，进行扫描

在**Controller的Java代码**中加入**注解**