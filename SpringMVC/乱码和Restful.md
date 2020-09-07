##### 1.乱码的解决-----通过过滤器来解决乱码SpringMVC中提供CharacterEncodingFilter 解决

解决Post乱码

```xml
  <filter>
  	<filter-name>CharacterEncodingFilter</filter-name>
  	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>utf-8</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>CharacterEncodingFilter</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

* 如果是get可以修改tomcat的配置文件

* 自定义乱码解决方案

##### 2.Restful风格（简单，高效，安全）

http://localhost:8080/07springmvc_codeRestful/1/2/delete.path

```java 
	@RequestMapping("/{id}/{uid}/delete")
	public String hello(@PathVariable("id") int id,@PathVariable("uid") int uid,ModelMap map) {
		System.out.println("id"+id+"uid"+uid);
		map.addAttribute("name", id+uid);
		return "go.jsp";
	}
```

##### 3.	@RequestMapping(params = "method=方法函数名",method = RequestMethod.请求方式)

http://localhost:8080/07springmvc_codeRestful/delete.path?method=方法名