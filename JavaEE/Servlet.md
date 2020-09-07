![image.png](https://upload-images.jianshu.io/upload_images/14935748-8912c0353784ee0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-97f7ddf039622dc4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###配置文件(xml解析后，通过类的反射，)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-e08b61bae48d4fbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-32f86aaaf55ab642.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
Class clazz = Class.forName("类名");//反射创建类
Method method[] = clazz.getMethods();//得到方法
method[1].invoke(obj, args);//调用方法
Object object = clazz.getDeclaredConstructor().newInstance();//实例化对象
等等....实现配置文件的响应

```
```
package servletA;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class Bservlet	implements Servlet {

	public static void main(String[] args) {
		System.out.println("mian()");
	}

	@Override
	public void destroy() {
		System.out.println("destroy()");
		
	}
@Override
	public ServletConfig getServletConfig() {
		System.out.println("getServletConfig()");
		return null;
	}
	@Override
	public String getServletInfo() {
		System.out.println("getServletInfo()");
		return null;
	}
	@Override
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init(ServletConfig config)");
		System.out.println(config.getInitParameter("n1"));//通过参数获取值
		System.out.println(config.getInitParameter("n2"));
		Enumeration enumeration = config.getInitParameterNames();//获取参数
		while (enumeration.hasMoreElements()) {
			System.out.println(enumeration.nextElement()); 	
		}
	}
	@Override
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
		System.out.println("service(ServletRequest req, ServletResponse res) ");	
	}
}
```

*********

#### 使用 Servlet，您可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。
#### Servlet接口包含的方法

```
@Override
	public void destroy() {
		System.out.println("destroy()");
		
	}

	@Override
	public ServletConfig getServletConfig() {
		System.out.println("getServletConfig()");
		return null;
	}

	@Override
	public String getServletInfo() {
		System.out.println("getServletInfo()");
		return null;
	}

	@Override
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init(ServletConfig config)");
		
	}

	@Override
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
		System.out.println("service(ServletRequest req, ServletResponse res) ");
		
	}
```
针对servlet的方法的多数都不是由我们调用的，而是有服务器调用 ，并且servlet；对象也不是我们创建的，通常也是服务器创建的，我们完成的是片段程序，
>Servlet 执行以下主要任务：
读取客户端（浏览器）发送的显式的数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。
读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。
处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应。
发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。



### init() 方法
init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用。因此，它是用于一次性初始化，就像 Applet 的 init 方法一样。
Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载。
当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。
init 方法的定义如下：

```
public void init() throws ServletException {
  // 初始化代码...}
```
### service() 方法
service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。

下面是该方法的特征：
```
public void service(ServletRequest request, 
                    ServletResponse response) 
      throws ServletException, IOException{}
```
service() 方法由容器调用，service 方法在适当的时候调用 doGet、doPost、doPut、doDelete 等方法。所以，您不用对 service() 方法做任何动作，您只需要根据来自客户端的请求类型来重写 doGet() 或 doPost() 即可。

###doGet() 方法
GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。
```
public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码}
```
### ##doPost() 方法
POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。

```
public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码}
```
##### destroy() 方法
destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。
在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收。destroy 方法定义如下所示：

```
 public void destroy() {
    // 终止化代码...
  }
```

#### servlet是单例的，是线程不安全的。



##### Servlet 表单数据
很多情况下，需要传递一些信息，从浏览器到 Web 服务器，最终到后台程序。浏览器使用两种方法可将这些信息传递到 Web 服务器，分别为 GET 方法和 POST 方法。

##### GET 方法
GET 方法向页面请求发送已编码的用户信息。页面和已编码的信息中间用 ? 字符分隔，如下所示：

```
http://www.test.com/hello?key1=value1&key2=value2
```
GET 方法是默认的从浏览器向 Web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。GET 方法有大小限制：请求字符串中最多只能有 1024 个字符。

这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 doGet() 方法处理这种类型的请求。

##### POST 方法
另一个向后台程序传递信息的比较可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出。Servlet 使用 doPost() 方法处理这种类型的请求。

使用 Servlet 读取表单数据
Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

getParameter()：您可以调用 request.getParameter() 方法来获取表单参数的值。

getParameterValues()：如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。

getParameterNames()：如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。
![image.png](https://upload-images.jianshu.io/upload_images/14935748-cad8483b6356c91a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-121cd307359d97eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####读取 HTTP 头的方法
下面的方法可用在 Servlet 程序中读取 HTTP 头。这些方法通过 HttpServletRequest 对象可用。

序号	方法 & 描述
1	Cookie[] getCookies()
返回一个数组，包含客户端发送该请求的所有的 Cookie 对象。
2	Enumeration getAttributeNames()
返回一个枚举，包含提供给该请求可用的属性名称。
3	Enumeration getHeaderNames()
返回一个枚举，包含在该请求中包含的所有的头名。
4	Enumeration getParameterNames()
返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。
5	HttpSession getSession()
返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。
6	HttpSession getSession(boolean create)
返回与该请求关联的当前 HttpSession，或者如果没有当前会话，且创建是真的，则返回一个新的 session 会话。
7	Locale getLocale()
基于 Accept-Language 头，返回客户端接受内容的首选的区域设置。
8	Object getAttribute(String name)
以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。
9	ServletInputStream getInputStream()
使用 ServletInputStream，以二进制数据形式检索请求的主体。
10	String getAuthType()
返回用于保护 Servlet 的身份验证方案的名称，例如，"BASIC" 或 "SSL"，如果JSP没有受到保护则返回 null。
11	String getCharacterEncoding()
返回请求主体中使用的字符编码的名称。
12	String getContentType()
返回请求主体的 MIME 类型，如果不知道类型则返回 null。
13	String getContextPath()
返回指示请求上下文的请求 URI 部分。
14	String getHeader(String name)
以字符串形式返回指定的请求头的值。
15	String getMethod()
返回请求的 HTTP 方法的名称，例如，GET、POST 或 PUT。
16	String getParameter(String name)
以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。
17	String getPathInfo()
当请求发出时，返回与客户端发送的 URL 相关的任何额外的路径信息。
18	String getProtocol()
返回请求协议的名称和版本。
19	String getQueryString()
返回包含在路径后的请求 URL 中的查询字符串。
20	String getRemoteAddr()
返回发送请求的客户端的互联网协议（IP）地址。
21	String getRemoteHost()
返回发送请求的客户端的完全限定名称。
22	String getRemoteUser()
如果用户已通过身份验证，则返回发出请求的登录用户，或者如果用户未通过身份验证，则返回 null。
23	String getRequestURI()
从协议名称直到 HTTP 请求的第一行的查询字符串中，返回该请求的 URL 的一部分。
24	String getRequestedSessionId()
返回由客户端指定的 session 会话 ID。
25	String getServletPath()
返回调用 JSP 的请求的 URL 的一部分。
26	String[] getParameterValues(String name)
返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。
27	boolean isSecure()
返回一个布尔值，指示请求是否使用安全通道，如 HTTPS。
28	int getContentLength()
以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。
29	int getIntHeader(String name)
返回指定的请求头的值为一个 int 值。
30	int getServerPort()
返回接收到这个请求的端口号。
31	int getParameterMap()
将参数封装成 Map 类型



##### Servlet 服务器 HTTP 响应
当一个 Web 服务器响应一个 HTTP 请求时，响应通常包括一个状态行、一些响应报头、一个空行和文档。一个典型的响应如下所示：
```
HTTP/1.1 200 OKContent-Type: text/htmlHeader2: ......HeaderN: ...
  (Blank Line)<!doctype ...><html><head>...</head><body>...</body></html>
```
状态行包括 HTTP 版本（在本例中为 HTTP/1.1）、一个状态码（在本例中为 200）和一个对应于状态码的短消息（在本例中为 OK）。

下表总结了从 Web 服务器端返回到浏览器的最有用的 HTTP 1.1 响应报头，您会在 Web 编程中频繁地使用它们：

头信息	描述
Allow	这个头信息指定服务器支持的请求方法（GET、POST 等）。
Cache-Control	这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：public、private 或 no-cache 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存。
Connection	这个头信息指示浏览器是否使用持久 HTTP 连接。值 close 指示浏览器不使用持久 HTTP 连接，值 keep-alive 意味着使用持久连接。
Content-Disposition	这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘。
Content-Encoding	在传输过程中，这个头信息指定页面的编码方式。
Content-Language	这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等。
Content-Length	这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息。
Content-Type	这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型。
Expires	这个头信息指定内容过期的时间，在这之后内容不再被缓存。
Last-Modified	这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 If-Modified-Since 请求头信息提供一个日期。
Location	这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档。
Refresh	这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数。
Retry-After	这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求。
Set-Cookie	这个头信息指定一个与页面关联的 cookie。


####设置 HTTP 响应报头的方法
下面的方法可用于在 Servlet 程序中设置 HTTP 响应报头。这些方法通过 HttpServletResponse 对象可用。

序号	方法 & 描述
1	String encodeRedirectURL(String url)
为 sendRedirect 方法中使用的指定的 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。
2	String encodeURL(String url)
对包含 session 会话 ID 的指定 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。
3	boolean containsHeader(String name)
返回一个布尔值，指示是否已经设置已命名的响应报头。
4	boolean isCommitted()
返回一个布尔值，指示响应是否已经提交。
5	void addCookie(Cookie cookie)
把指定的 cookie 添加到响应。
6	void addDateHeader(String name, long date)
添加一个带有给定的名称和日期值的响应报头。
7	void addHeader(String name, String value)
添加一个带有给定的名称和值的响应报头。
8	void addIntHeader(String name, int value)
添加一个带有给定的名称和整数值的响应报头。
9	void flushBuffer()
强制任何在缓冲区中的内容被写入到客户端。
10	void reset()
清除缓冲区中存在的任何数据，包括状态码和头。
11	void resetBuffer()
清除响应中基础缓冲区的内容，不清除状态码和头。
12	void sendError(int sc)
使用指定的状态码发送错误响应到客户端，并清除缓冲区。
13	void sendError(int sc, String msg)
使用指定的状态发送错误响应到客户端。
14	void sendRedirect(String location)
使用指定的重定向位置 URL 发送临时重定向响应到客户端。
15	void setBufferSize(int size)
为响应主体设置首选的缓冲区大小。
16	void setCharacterEncoding(String charset)
设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。
17	void setContentLength(int len)
设置在 HTTP Servlet 响应中的内容主体的长度，该方法设置 HTTP Content-Length 头。
18	void setContentType(String type)
如果响应还未被提交，设置被发送到客户端的响应的内容类型。
19	void setDateHeader(String name, long date)
设置一个带有给定的名称和日期值的响应报头。
20	void setHeader(String name, String value)
设置一个带有给定的名称和值的响应报头。
21	void setIntHeader(String name, int value)
设置一个带有给定的名称和整数值的响应报头。
22	void setLocale(Locale loc)
如果响应还未被提交，设置响应的区域。
23	void setStatus(int sc)
为该响应设置状态码。



####servletContext一个项目就只有一个，是所有的servlet的数据传递员｛与天地同寿---与服务器同生共死｝
>######辨析WebApplicationContext, ApplicationContext, ServletContext以及ServletConfig

>ServletContext: 这个是来自于servlet规范里的概念，它是servlet用来与容器间进行交互的接口的组合，也就是说，这个接口定义了一系列的方法，servlet通过这些方法可以很方便地与自己所在的容器进行一些交互，比如通过getMajorVersion与getMinorVersion来获取容器的版本信息等. 从它的定义中也可以看出，在一个应用中(一个JVM)只有一个ServletContext, 换句话说，容器中所有的servlet都共享同一个ServletContext.

>ServletConfig: 它与ServletContext的区别在于，servletConfig是针对servlet而言的，每个servlet都有它独有的serveltConfig信息，相互之间不共享.

>ApplicationContext: 这个类是Spring实现容器功能的核心接口，它也是Spring实现IoC功能中最重要的接口，从它的名字中可以看出，它维护了整个程序运行期间所需要的上下文信息， 注意这里的应用程序并不一定是web程序，也可能是其它类型的应用. 在Spring中允许存在多个applicationContext，这些context相互之间还形成了父与子，继承与被继承的关系，这也是通常我们所说的，在spring中存在两个context,一个是root context，一个是servlet applicationContext的意思. 这点后面会进一步阐述.

>WebApplicationContext: 其实这个接口不过是applicationContext接口的一个子接口罢了，只不过说它的应用形式是web罢了. 它在ApplicationContext的基础上，添加了对ServletContext的引用，即getServletContext方法.

```
//获取servletContext对象  的两种方式
		ServletContext context1=this.getServletContext();//最常用
		ServletContext context2=this.getServletConfig().getServletContext();
```

####ServletContext源码
```
public interface ServletContext {
    public String getContextPath();

    public ServletContext getContext(String uripath);

    public int getMajorVersion();
    public int getMinorVersion();
    public int getEffectiveMajorVersion();
    public int getEffectiveMinorVersion();
    
    public String getServerInfo();
    public String getServletContextName();

    public String getMimeType(String file);
    
    public void log(String msg);
    public void log(String message, Throwable throwable);

    public Set<String> getResourcePaths(String path);
 
    public URL getResource(String path) throws MalformedURLException;
    public InputStream getResourceAsStream(String path);
    
    public RequestDispatcher getRequestDispatcher(String path);
    public RequestDispatcher getNamedDispatcher(String name);

    public String getRealPath(String path);

    public String getInitParameter(String name);
    public Enumeration<String> getInitParameterNames();
    public boolean setInitParameter(String name, String value);

    public Object getAttribute(String name);
    public Enumeration<String> getAttributeNames();
    public void setAttribute(String name, Object object);
    public void removeAttribute(String name);

    // 省略部分方法
}
```
####网站访问次数统计

```
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {//GET方法
		ServletContext context = this.getServletContext();
		Integer count = (Integer) context.getAttribute("count");
		if(count==null) {
			context.setAttribute("count", 1);
		}
		else {
			context.setAttribute("count", count+1);
		}
		PrintWriter pWriter = response.getWriter();
		pWriter.print(count);

		
	}
```

#####classLoader加载器加载资源文件
```
    /*
     * 输入流转换为字符串
     */
    public static String getStringFromInputStream(InputStream inputStream) {
        String resultData = null;      //需要返回的结果
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        byte[] data = new byte[1024];
        int len = 0;
        try {
            while((len = inputStream.read(data)) != -1) {
                byteArrayOutputStream.write(data, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        resultData = new String(byteArrayOutputStream.toByteArray());    
        return resultData;
    }


	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
		/*
		 * 得到classLoader
		 *  >先得到Class在得到classLoader
		 * 得到getResourceAsStream,得到一个Inputtream
		 */
		ClassLoader loader = this.getClass().getClassLoader();//获取当前类并且得到资源加载器
		InputStream ioStream = loader.getResourceAsStream("a.txt");//获取资源并转换为输入流
		String string = getStringFromInputStream(ioStream);//输入流转换成字符串
		PrintWriter printWriter = response.getWriter();
		printWriter.print(string);
		
	}
```
```
	        /*
		 * 导入common-io包实现流的转换
		 * IOUtils.toString(ioStream);
		 */
		 StringWriter writer = new StringWriter(); 
		 IOUtils.copy(ioStream, writer, "utf-8"); 
		 String theString = writer.toString();
```

####service服务调用
![image.png](https://upload-images.jianshu.io/upload_images/14935748-86b91996144eae39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-f0b133cd2238eb1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-e8c8fa5eb5e00f84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
package servletA;

import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class BaseServlet
 */
@WebServlet("/BaseServlet")
public class BaseServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public BaseServlet() {
        super();
    }
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	super.service(request, response);
    	String methodName = request.getParameter("method");//获取方法名
    	if(methodName == null||methodName.trim().isEmpty()) {
    		System.err.println("excpetion");
    		
    	}
    	/*
    	 * 得到方法名
    	 * 反射实现类的创建，通过类实现方法的调用
    	 */
    	Class clazz = this.getClass();
    	Method method = null;
    	try {
			 method = clazz.getMethod(methodName, HttpServletRequest.class,HttpServletResponse.class);//利用反射获取方法
			
		} catch (Exception e) {
			e.printStackTrace();
		} 
    	try {
			method.invoke(this,request,response);
		} catch (Exception e) {
			e.printStackTrace();
		}
    	
    
    	
    }
    /*service调用的方法
     * 一定要定义为public方法
     */
    public void addUser(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	System.out.println("adduser");
    	
    }
    public void editeUser(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	System.out.println("editeuser");
    }
    public void delUser(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	System.out.println("deluser");
    }
    /*
     * 
     */
    
    
    /*
     * 默认请求响应方法
     */
    
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```

