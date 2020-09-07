EL做到全域查找
```
${ 表达式}
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
<%
pageContext.setAttribute("xxx", "page");
request.setAttribute("xxx", "request");
session.setAttribute("xxx", "session");
application.setAttribute("xxx", "APP");

%>
${xxx}<br>
${pageScope.xxx}<br>
${requestScope.xxx}<br>
${sessionScope.xxx}<br>
${applicationScope.xxx}<br>
<!-- 输出结果 -->
<!-- page -->
<!-- page -->
<!-- request -->
<!-- session-->
<!-- APP-->

</body>
</html>
```

>javabean导航
![image.png](https://upload-images.jianshu.io/upload_images/14935748-f7dc051ca629e473.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
只要满足了JavaBean规范的任意的属性都可以直接调用也即是说EL表达式相当于调用JavaBean的所有Get方法

###JSTL
![image.png](https://upload-images.jianshu.io/upload_images/14935748-dbce61cb664a665d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
