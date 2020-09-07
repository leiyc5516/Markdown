>ajax 是什么：
1.（asynchsronous javascript and xml）异步的js和xml
2.他能使用js访问服务器，而且是异步访问！
3.服务器给客户端，的响应一般是一个完整的html!但是ajax中因为是局部刷新那么服务器将不再需要响应整个页面，而是数据：
                        包括纯文本text
                    也可以是xml
              还可以是json:最受欢迎

>同步：
1.发送请求，要等到服务器响应结束，才可以发送第二个请求！中间不可以处理别的请求。
2.请求响应刷新整个页面
>异步：
1.发送一个请求后无需等待服务器响应，然后就可以发送第二个请求！
2.可以使用JS接受服务器的响应，然后使用JS局部刷新。



>ajax应用场景：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-862e9b2aa32b3a53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-95591dc3426624c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>ajax优缺点：
1.优点;
异步交互用户的体验增加
性能：不需要刷新整个页面，只需要响应部分内容服务器的压力减轻了
2.缺点:
ajax不能运用在所有场景
ajax增加了 服务器访问的次数增加了服务器压力

>ajax GET发送异步请求四大步骤
1.得到XMLHttpRequest对象
ajax只需要只需要学习一个对象：XMLHttpRequest,如果掌握了它就掌握了ajax
```
var xmlHttp = new XMLHttpRequest();//大多数浏览器都支持 
var xmlHttp = new ActiveXObject("Msxml2.XMLHTTP ");IE6.0
var xmlHttp = new ActiveXObject("Microsoft.XMLHTTP ");IE5.5及以前版本的IE浏览器

```
编写创建XMLHttpRequest对象的函数
```
function createXMLHttpRequest(){
	try {
		return new function XMLHttpRequest();
	} catch (e) {
		try {
			return new ActiveXObject("Msxml2.XMLHTTP ");
		} catch (e) {
			try {
				new ActiveXObject("Microsoft.XMLHTTP ");	
			} catch (e) {
				alert("无匹配的浏览器")；
				throw e;
			}
		}
	}
}

```

2.打开于服务器的连接
```
xmlHttp.open();//用来打开服务器建立连接
/***
三个参数：
请求方式：可以是GET或者POST；
请求的URL：指定服务器端资源
请求是否异步：true表示发送异步请求，否者同步请求
例如：xmlHttp.open("GET",  "/Ajax/day1", true);
***/
```
3.发送请求
```
xmlHttp.send（null）;//GET可以给null ,但是POST需要发送请求实体；
```
GET不需要参数,但是POST还需要参数，即是请求体

4.
*  在xmlHttp上注册监听器：onreadystatechange
*  xmlHttp对象五个状态：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-828b7ecefe0b9d13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
*  得到xmlHttp.readyState
```
var state = xmlHttp.readyState;//可能是0，1，2，3，4
```
*  得到服务器状态码
```
var  status =  xmlHttp.readystatus;//例如200，404，502等
```
*   得到服务器响应的内容
```
var content = xmlHttp.responseText;//得到服务器响应的纯文本内容
var content = xmlHttp.responseXML;//得到服务器响应的XML内容,他是一个document对象
```
*
```
xmlHttp.onreadyststechange = function (){
	if((xmlHttp.readyState == 4) && (xmlHttp.status == 200)) {//判断是否是响应结束，响应正常
		var text =  xmlHttp.responseText;//更加常用
                var content = xmlHttp.responseXML;
	}
}
```


案例：

服务器端
```


import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloWorld
 */
@WebServlet(asyncSupported = true, urlPatterns = { "/HelloWorld" })
public class HelloWorld extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloWorld() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("Hello ajax!");
		response.getWriter().print("Hello ajax!!!");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

客户端
·
```
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
<script type="text/javascript">
function createXMLHttpRequest(){//针对使用的不同浏览器使用不同的对象创建方式
	try {
		return new  XMLHttpRequest();
	} catch (e) {
		try {
			return new ActiveXObject("Msxml2.XMLHTTP ");
		} catch (e) {
			try {
				new ActiveXObject("Microsoft.XMLHTTP ");	
			} catch (e) {
				alert("无匹配的浏览器");
			}
		}
	}
}
window.onload = function () {//文档加载完毕后执行
	var btn = document.getElementById("btn");//获取元素
	btn.onclick = function () {//ajax四步操作，把服务器响应的内容嵌入到H1标签中
		
		var xmlHttp = createXMLHttpRequest();//得到xmlHttp对象
		xmlHttp.open('GET', '/Ajax/HelloWorld', true);//用来打开服务器建立连接
		xmlHttp.send(null);//GET可以给null ,但是POST需要发送请求实体；
		xmlHttp.onreadystatechange = function (){
			if((xmlHttp.readyState == 4) && (xmlHttp.status == 200)) {//判断是否是响应结束，响应正常
				var text =  xmlHttp.responseText;//更加常用
				var h1 = document.getElementById("h1");
				h1.innerHTML = text;
			};
		}
	
	}
}
</script>
</head>
<body>

<button id = "btn">点击这里</button>
<h1 id =  "h1">
</h1>

</body>
</html>
```
项目包结构：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-9ac7ad891899ec45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

效果图：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-34ee91124fe73f7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-660ed69e9a641cd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





