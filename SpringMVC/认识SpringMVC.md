## Spring MVC

Spring框架提供了构建Web应用程序的全功能MVC模块，即Spring MVC
Spring MVC框架，提供了一个DispatcherServlet，作为前端控制器来分派请求，同时，提供了灵活的配置处理程序映射、视图解析、语言环境和主题解析，并支持文件上传

## SpringMVC的优点

1.清晰的角色划分：控制器(controller)、验证器(validator)、命令对象(command obect)、表单对象(form object)、模型对象(model object)、Servlet分发器(DispatcherServlet)、处理器映射(handler mapping)、试图解析器(view resoler)等等。每一个角色都可以由一个专门的对象来实现。

2.强大而直接的配置方式：将框架类和应用程序类都能作为JavaBean配置，支持跨多个context的引用，例如，在web控制器中对业务对象和验证器validator)的引用。

3.可适配、非侵入：可以根据不同的应用场景，选择何事的控制器子类(simple型、command型、from型、wizard型、multi-action型或者自定义)，而不是一个单一控制器(比如Action/ActionForm)继承。

4.可重用的业务代码：可以使用现有的业务对象作为命令或表单对象，而不需要去扩展某个特定框架的基类。

5.可定制的绑定(binding)和验证(validation)：比如将类型不匹配作为应用级的验证错误，这可以保证错误的值。再比如本地化的日期和数字绑定等等。在其他某些框架中，你只能使用字符串表单对象，需要手动解析它并转换到业务对象。

6.可定制的handler mapping和view resolution：Spring提供从最简单的URL映射，到复杂的、专用的定制策略。与某些web MVC框架强制开发人员使用单一特定技术相比，Spring显得更加灵活。

7.灵活的model转换：在Springweb框架中，使用基于Map的键/值对来达到轻易的与各种视图技术集成。

8.可定制的本地化和主题(theme)解析：支持在JSP中可选择地使用Spring标签库、支持JSTL、支持Velocity(不需要额外的中间层)等等。

9.简单而强大的JSP标签库(Spring Tag Library)：支持包括诸如数据绑定和主题(theme)之类的许多功能。他提供在标记方面的最大灵活性。

10.JSP表单标签库：在Spring2.0中引入的表单标签库，使用在JSP编写表单更加容易。

11.Spring Bean的生命周期：可以被限制在当前的HTTp Request或者HTTp Session。准确的说，这并非Spring MVC框架本身特性，而应归属于Spring MVC使用的WebApplicationContext容器。

 

------



##### 1.MVC框架需要做的事情：

* 将一个url映射到Java类或者Java方法 。
* 封装用户提交数据
* 处理用户请求，相关业务处理
* 将响应数据封装
* 将响应数据渲染，可以渲染成jsp,html,或者freemarker等

##### 2.SpringMVC：

是一个轻量级（没有侵入性质的）的**基于请求响应**的MVC框架。

**对应的JSF:是基于时间驱动的**

##### 3.为什么要使用SpringMVC

* 性能比Struts2好，Struts2开发起来方便 
* 便捷简单，开发起来更加简单，容易学
* 天生和Spring无缝集成  （使用SpringIOC，AOP）
* 使用约定优于配置
* 能够简单的Junit测试
* 支持Restful风格
* 本地化国际化
* 数据验证，类型转换
* 拦截器
* 使用的人多

##### SpringMVC结构

![img](http://www.yiibai.com/uploads/tutorial/20160116/1-1601161F914292.png)

<img src="E:\MarkdownPicture\SpringMVC.png" alt="SpringMVC" style="zoom: 200%;" />