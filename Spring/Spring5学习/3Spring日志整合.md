### 第三章Spring5.X与日志框架整合

>Spring与日志框架进行整合，日志框架就可以在控制台中，输出Spring运行过程的一些核心的信息
>
>好处：便于了解Spring的运行过程，利于程序调试

####  1.Spring如何整合框架

~~~markdown
默认：
    1.早期的Spring版本，Spring1~3整合的是commons-logging.jar
    2.Spring5.x默认整合的日志框架是logback，或者是log4j2
Spring5.x整合log4j
	1.引入log4j.jar
	2.导入log4j.properties配置文件
~~~

在pom文件中需要引入

~~~xml
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.25</version>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
~~~

引入log4j.properties

~~~properties

log4j.rootLogger=INFO,CONSOLE
log4j.addivity.org.apache=true
 
# console
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Threshold=INFO
log4j.appender.CONSOLE.Target=System.out
log4j.appender.CONSOLE.Encoding=UTF-8
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=[demo] %-5p %d{yyyy-MM-dd HH\:mm\:ss} - %C.%M(%L)[%t] - %m%n
 
# all
log4j.logger.com.demo=INFO, DEMO
log4j.appender.DEMO=org.apache.log4j.RollingFileAppender
log4j.appender.DEMO.File=${catalina.base}/logs/demo.log
log4j.appender.DEMO.MaxFileSize=50MB
log4j.appender.DEMO.MaxBackupIndex=3
log4j.appender.DEMO.Encoding=UTF-8
log4j.appender.DEMO.layout=org.apache.log4j.PatternLayout
log4j.appender.DEMO.layout.ConversionPattern=[demo] %-5p %d{yyyy-MM-dd HH\:mm\:ss} - %C.%M(%L)[%t] - %m%n
~~~





