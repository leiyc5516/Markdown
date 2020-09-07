

## springMVC原理解析

1：SpringMVC运行原理

![img](https://img2018.cnblogs.com/blog/1385231/201809/1385231-20180917095616600-968950504.png)

2：工作流程

　　（1）客户端（浏览器）发送请求，直接请求到DispatcherServlet。

　　（2）DispatcherServlet根据请求信息调用HandlerMapping，解析请求对应的Handler。

　　（3）解析到对应的Handler后，开始由HandlerAdapter适配器处理。

　　（4）HandlerAdapter会根据Handler来调用真正的处理器开处理请求，并处理相应的业务逻辑。

　　（5）处理器处理完业务后，会返回一个ModelAndView对象，Model是返回的数据对象，View是个逻辑上的View。

　　（6）ViewResolver会根据逻辑View查找实际的View。

　　（7）DispaterServlet把返回的Model传给View。

　　（8）通过View返回给请求者（浏览器）

 

　　

### SpringMVC流程

　

　　1、 用户发送请求至前端控制器DispatcherServlet。

　　2、 DispatcherServlet收到请求调用HandlerMapping处理器映射器。

　　3、 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

　　4、 DispatcherServlet调用HandlerAdapter处理器适配器。

　　5、 HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

　　6、 Controller执行完成返回ModelAndView。

　　7、 HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。

　　8、 DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

　　9、 ViewReslover解析后返回具体View。

　　10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。

　　11、 DispatcherServlet响应用户。

　　

 

3：主要组件

#### 　　前端控制器（DisatcherServlet）:接收请求，响应结果，返回可以是json,String等数据类型，也可以是页面（Model）。

#### 　　处理器映射器（HandlerMapping）:根据URL去查找处理器，一般通过xml配置或者注解进行查找。

#### 　　处理器（Handler）：就是我们常说的controller控制器啦，由程序员编写。

#### 　　处理器适配器（HandlerAdapter）:可以将处理器包装成适配器，这样就可以支持多种类型的处理器。

#### 　　视图解析器（ViewResovler）:进行视图解析，返回view对象（常见的有JSP,FreeMark等）。

4：　组件说明

　　**1、前端控制器DispatcherServlet（不需要工程师开发）,由框架提供**
　　作用：接收请求，响应结果，相当于转发器，中央处理器。有了dispatcherServlet减少了其它组件之间的耦合度。
　　用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

　　**2、处理器映射器HandlerMapping(不需要工程师开发),由框架提供**
　　作用：根据请求的url查找Handler
　　HandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

　　**3、处理器适配器HandlerAdapter**
　　作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler
　　通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

　　**4、处理器Handler(需要工程师开发)**
　　**注意：编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler**
　　Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。
　　由于Handler涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发Handler。

　　**5、视图解析器View resolver(不需要工程师开发),由框架提供**
　　作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）
　　View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。 springmvc框架提供了很多的View视图类型，包括：jstlView、　      freemarkerView、pdfView等。
    一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由工程师根据业务需求开发具体的页面。

　　**6、视图View(需要工程师开发jsp...)**
　　View是一个接口，实现类支持不同的View类型（jsp、freemarker、pdf...）