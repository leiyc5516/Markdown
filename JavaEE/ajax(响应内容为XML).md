![image.png](https://upload-images.jianshu.io/upload_images/14935748-9eca19fd1b529815.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
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
		xmlHttp.open('GET', '/Ajax/Aservlet', true);//用来打开服务器建立连接
		xmlHttp.send(null);//GET可以给null ,但是POST需要发送请求实体；
		xmlHttp.onreadystatechange = function (){
			if((xmlHttp.readyState == 4) && (xmlHttp.status == 200)) {//判断是否是响应结束，响应正常
				var doc =  xmlHttp.responseXML;//XML传输实现，得到document对象
				/****
				解析XML传输后的Document对象
				****/
				//待完善
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

服务器
```


import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class Aservlet
 */
@WebServlet("/Aservlet")
public class Aservlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Aservlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
		response.setContentType("text/xml;charset=utf-8");
		request.setCharacterEncoding("utf-8");//处理中文
		String  xmlString ="<note>" + 
				"<to>George</to>" + 
				"<from>John</from>" + 
				"<heading>Reminder</heading>" + 
				"<body>Don't forget the meeting!</body>" + 
				"</note>";
		response.getWriter().print(xmlString);

	}

}
```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-c6f80682c1f501b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
