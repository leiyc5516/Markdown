![image.png](https://upload-images.jianshu.io/upload_images/14935748-abb623f9f8a5d8b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>向Session保存数据</h1>
<%session.setAttribute("leiyc", "55160831") ;%>

</body>
</html>
```

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>获取Session数据</h1>
<%String str=(String)session.getAttribute("leiyc"); %>
<%=str %>

</body>
</html>
```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-2476e67f3cec8855.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/14935748-984aca8b67285dd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

登录页
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<% 
	String msg =" ";
	if((String)request.getAttribute("msg")!=null){
		msg= (String)request.getAttribute("msg"); 
	}
%>
<%
/*
*读取cookie的数据
*/
String message = "";
Cookie cookie[] = request.getCookies();
if(cookie!=null)
	for(Cookie c: cookie){
	if("username".equals(c.getName()))
		message = c.getValue();
	}
%>
<h1>登录</h1>
<span><%=msg %></span>
<form action="/Session/LoginServlet" method="post" id="formid">
账号：<input type="text" id="num" name="username" value="<%=message%>">
密码：<input type="password" id="pas" name="password">
<input type="submit" id="sub" value = "登录">
</form>

</body>
</html>
```

成功页
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>succ2</title>
</head>
<body>
<%
session = request.getSession() ;
String str = (String)session.getAttribute("username");
if(str==null){
	/*
	*向request保存信息，转发到login.jsp
	*/
	request.setAttribute("msg", "请先登录");
	request.getRequestDispatcher("/login.jsp").forward(request, response);
	return;
}
%>
	<h1>欢迎<%=str %></h1>
</body>
</html>
```
服务器
```
package com;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;



@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
    public LoginServlet() {
        super();
       
    }


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	
		doGet(request, response);
		request.setCharacterEncoding("utf-8");
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		System.err.println(username);
		System.err.println(password);
		if ((username.equalsIgnoreCase("leiyc"))&&(password.equalsIgnoreCase("123"))) {//登陆成功
			
			/*
			 * 利用cookie保存用户名发送给浏览器客户端
			 */
			Cookie cookie = new Cookie("username",username);
			cookie.setMaxAge(60*60);
			response.addCookie(cookie);
			
			
			
			/*
			 * 登录成功
			 * 保存用户信息到Session
			 * 重定向到succ1.jsp
			 */
			
			HttpSession session = request.getSession();//获取当前Session
			session.setAttribute("username", username);//向Session域中保存用户信息
			response.sendRedirect("/Session/succ1.jsp");
		}
		else {
			/*
			 * 登录失败
			 * 保存错误信息到requst域中转达到login.jsp
			 * 
			 */
			System.err.println(password);
			request.setAttribute("msg", "用户名或者密码错误");
			RequestDispatcher rd = request.getRequestDispatcher("/login.jsp");//得到转发器
			rd.forward(request, response);//转发
		}
		
			
	}
}


```

![image.png](https://upload-images.jianshu.io/upload_images/14935748-0d83c05dde46630d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-63725df479bedfa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-8e3bcdcf98b973c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
<a href="/Session/login.jsp;JSESSIONID<%=session.getId() %>">点击这里</a>//注意分号，获得SessionID
```

```
response.encodeURL("/Sesson/login.jsp");//重写URL
```

