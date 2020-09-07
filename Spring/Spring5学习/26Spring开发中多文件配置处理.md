~~~mariadb
Spring 会根据需要，把相关配置文件信息分配到不同的文件中，便于后续的管理和维护


DAO-------- applicationContext-dao.xml
Service ------applicationContext-Service.xml
Action------applicationContext-Action.xml


注意：虽然提供多个配置文件，但是后续需要整合到一起
	* 通配符方式
		* 非web环境下
			* applicationContext ctx = new 				   ClassApplicationContext("applicationContext-*.xml")
		* 在web环境下
			*<context-param>
				<param-name>contextConfigLoaction</param-name>
				<param-value>classpath:applicationContext-*.xml</param-value>
			<context-param>
			
	* <import>标签
		*applicationContext.xml 目的是整合其他配置内容
			* <import resource=" applicationContext-dao.xml">
			* <import resource=" applicationContext-Service.xml">
			* <import resource="applicationContext-Action.xml">
		*非web环境下
		applicationContext ctx = new 				   ClassApplicationContext("applicationContext.xml")
		* 在web环境下
			*<context-param>
				<param-name>contextConfigLoaction</param-name>
				<param-value>classpath:applicationContext.xml</param-value>
			<context-param>
~~~

