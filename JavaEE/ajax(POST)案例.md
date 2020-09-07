![image.png](https://upload-images.jianshu.io/upload_images/14935748-a0f79d503588c35d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>ajax POST发送异步请求五步骤
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
3.设置请求头Content-Type
```
xmlHttp.setRequestHeader('Content-Type' , 'application/x-www-form-urlencoded');
```

4.发送请求
```
xmlHttp.send('username=zhangsan&password=123');//POST需要发送请求实体,等号左右无空格；
```
GET不需要参数,但是POST还需要参数，即是请求体

5.
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
服务器：
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
		response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");//处理中文
		doGet(request, response);
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		System.out.println("(post)Hello ajax!"+username+password);
		response.getWriter().print("来了老弟!!!"+username+password);
	}

}


```

客户端：
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
		xmlHttp.open('POST', '/Ajax/HelloWorld', true);//用来打开服务器建立连接
		xmlHttp.setRequestHeader("Content-Type" , "application/x-www-form-urlencoded");//设置请求头
		xmlHttp.send("username=zhangsan&password=123");//但是POST需要发送请求实体；
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

