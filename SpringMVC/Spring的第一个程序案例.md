##### hello world 案例

步骤：

* 导入相关Jar包
* 
* 配置web.xml 文件----配置分发器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>01springmvc_helloworld</display-name>
  <servlet>
  	<servlet-name>SpringMVC</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>SpringMVC</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

类似servlet的配置

* 添加springmvc配置文件，默认在WEB-INF下面添加【servlet-name】-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:p="http://www.springframework.org/schema/p"
 xmlns:context="http://www.springframework.org/schema/context"
 xsi:schemaLocation="
 http://www.springframework.org/schema/beans
 https://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/context
 https://www.springframework.org/schema/context/spring-context.xsd">
 <context:component-scan base-package="org.springframework.samples.petclinic.web"/>
 <!-- ... -->
</beans>
```

* 编写HelloController.java

```java
package cn.sxt.hello;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class HelloController implements Controller {

	@Override
	public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
		ModelAndView modelAndView = new ModelAndView();
		//封装要显示到视图的数据 
		modelAndView.addObject("msg", "hello SpringMVC");
		//视图名称
		/*
		 *  <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		 *		 <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
		 *		  <!-- 结果视图前缀 -->
		 *		 <property name="prefix" value="/WEB-INF/jsp/"/>
		 *		  <!-- 结果视图后缀  -->
		 *		 <property name="suffix" value=".jsp"/>
 		 *	</bean>
		 */
		modelAndView.setViewName("hello");//渲染后的结果存放为    WEB-INF/jsp/hello.jsp
		
		return modelAndView;
	}

}

```

* 编写SpringMVC-servlet.xml配置文件

   ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:p="http://www.springframework.org/schema/p"
   xmlns:context="http://www.springframework.org/schema/context"
   xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   https://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   https://www.springframework.org/schema/context/spring-context.xsd">
   <context:component-scan base-package="org.springframework.samples.petclinic.web"/>
   <!-- 配置HandlerMapping -->
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
   </bean>
   <!-- 配置handlerAdapter -->
   <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter">
   </bean>
   <!-- 配置渲染器 -->
   <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  	 <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
  	  <!-- 结果视图前缀 -->
  	 <property name="prefix" value="/WEB-INF/jsp/"/>
  	  <!-- 结果视图后缀  -->
  	 <property name="suffix" value=".jsp"/>
   </bean>
    <!-- 配置请求和处理器 -->
    <bean name="/hello.do" class="cn.sxt.hello.HelloController"></bean>
  </beans>
  ```

  ```jsp
  <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
      pageEncoding="ISO-8859-1"%>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="ISO-8859-1">
  <title>Insert title here</title>
  </head>
  <body>
  	${msg}
  </body>
  </html>
  ```
  
  

出现的问题

记得导入

jstl-1.2.jar包

访问路径

http://localhost:8080/01springmvc_helloworld1/hello.do