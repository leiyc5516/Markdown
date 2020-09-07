>动态包含与静态包含的区别
1.两者格式不同，静态包含：<%@ include file="文件" %>，而动态包含：<jsp : include page = "文件" />。
2.包含时间不同，静态包含是先将几个文件合并，然后再被编译，缺点就是如果含有相同的标签，会出错。动态包含是页面被请求时编译，将结果放在一个页面。
3.生成的文件不同，静态包含会生成一个包含页面名字的servlet和class文件；而动态包含会各自生成对应的servlet和class文件
4.传递参数不同，动态包含能够传递参数，而静态包含不能


>JavaWeb一共提供了20个JSP动作标签

[常用动作元素的学习](https://www.runoob.com/jsp/jsp-actions.html)

### include标签

用来动态包含文件。

```
<jsp:include page="包含文件url地址" flush="true | false" />

```

### forward标签

forward标签用来转发用户的请求，使得用户请求一个页面跳转到另一个页面。这种跳转为服务器端转发，**用户的地址栏不会发生任何变化。**

```
<jsp:forward page="目标地址"  />

```

### param标签

param标签以"键/值"对的形式传递参数数据。

```
  <jsp:param name="参数名" value="参数值"/>

```

## useBean标签

`<jsp:useBean>`标签用于在指定的域范围内查找具有指定名称的对象。如果存在则返回该对象的引用，不存在则实例化一个新的对象并将它以指定的名称存储到指定的域范围中。

```
<jsp:useBean id="对象名称" scope="储存范围" class="类名"></jsp:useBean>
id属性：对象名称。
scope属性：对象储存范围。分别是page、request、session和application，默认page。
class属性：类名，必须是全限定名。
<jsp:useBean id="user" class="com.web.model.Users" scope="session"></jsp:useBean>

```

## setProperty标签

`<jsp:setProperty>`标签用于设置对象属性，相当于调用属性的setXxx方法。

```
<jsp:setProperty name="对象名" property="属性名称" value="属性值" />
name属性：对象名，和<jsp:useBean>中的id属性保持一致。
property属性：对象的属性名称。
value属性：属性值。
<jsp:setProperty property="name" name="user" value="Jevon" />

```

## getProperty标签

`<jsp:getProperty>`标签用于获得对象属性并输出到浏览器，相当于调用属性的getXxx方法。

```
<jsp:getProperty name="对象名" property="属性名称"/>
name属性：对象名，和<jsp:useBean>中的id属性保持一致。
property属性：属性名称。
<jsp:getProperty property="name" name="user"/>

```

