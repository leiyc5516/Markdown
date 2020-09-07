>XMLHttpRequest 对象用于在后台与服务器交换数据。
[XMLHttpRequest与异步请求](https://segmentfault.com/a/1190000011294052)
[XMLHttpReqeust与ajax1](https://www.jianshu.com/p/06b4b48a2330)
[XMLHttpReqeust与ajax2](https://www.jianshu.com/p/172899d7f2e7)

****
【创建一个 XMLHttpRequest 对象】


#### GET
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

****


####POST
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

*****
#####解析 XML 字符串
下面的代码片段把 XML 字符串解析到 XML DOM 对象中：
```
txt="<bookstore><book>";
txt=txt+"<title>Everyday Italian</title>";
txt=txt+"<author>Giada De Laurentiis</author>";
txt=txt+"<year>2005</year>";
txt=txt+"</book></bookstore>";

if (window.DOMParser)
{
parser=new DOMParser();
xmlDoc=parser.parseFromString(txt,"text/xml");
}
else // Internet Explorer
{
xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
xmlDoc.async=false;
xmlDoc.loadXML(txt); 
}
```


####案例1：
```
<!DOCTYPE html>
<html>
<body>
<h1>W3Cschool Internal Note</h1>
<div>
<b>To:</b> <span id="to"></span><br>
<b>From:</b> <span id="from"></span><br>
<b>Message:</b> <span id="message"></span>
</div>

<script>
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.open("GET","note.xml",false);
xmlhttp.send();
xmlDoc=xmlhttp.responseXML;

document.getElementById("to").innerHTML=
xmlDoc.getElementsByTagName("to")[0].childNodes[0].nodeValue;
document.getElementById("from").innerHTML=
xmlDoc.getElementsByTagName("from")[0].childNodes[0].nodeValue;
document.getElementById("message").innerHTML=
xmlDoc.getElementsByTagName("body")[0].childNodes[0].nodeValue;
</script>

</body>
</html>
```
####note.xml
```
<!--   Copyright w3school.com.cn  -->
<note time="16:06:03">
<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note>
```
