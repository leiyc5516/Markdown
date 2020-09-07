![image.png](https://upload-images.jianshu.io/upload_images/14935748-79acecbbabc14584.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-a2a8972e96c62eaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>保存cookie
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
	<h1>保存cookie</h1>
	<%
	Cookie cookie1 = new Cookie("leiyc","55160831");
	response.addCookie(cookie1);
	
	Cookie cookie2 = new Cookie("lwx","55160731");
	response.addCookie(cookie2);
	%>
</body>
</html>
```

>获取cookie
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
<h1>获取cookie</h1>
<%
Cookie cookies[] = request.getCookies();
if(cookies != null){
	for(Cookie c : cookies)
		out.print(c.getName()+ "="+c.getValue()+"</br>");
}
%>

</body>
</html>
```
>设置cookie的存活时间
```
<%
	Cookie cookie1 = new Cookie("leiyc","55160831");
	cookie1.setMaxAge(60*60);//设置cookie存活时间
	response.addCookie(cookie1);
	%>
````
