>[摘抄于w3c：XML命名空间](https://www.w3cschool.cn/xml/xml-namespaces.html)
####使用前缀来避免命名冲突
在 XML 中的命名冲突可以通过使用名称前缀从而容易地避免。
该 XML 携带某个 HTML 表格和某件家具的信息： 
```
<h:table>
<h:tr>
<h:td>Apples</h:td>
<h:td>Bananas</h:td>
</h:tr>
</h:table>

<f:table>
<f:name>African Coffee Table</f:name>
<f:width>80</f:width>
<f:length>120</f:length>
</f:table>
```

****
#####XML 命名空间 - xmlns 属性
当在 XML 中使用前缀时，一个所谓的用于前缀的命名空间必须被定义。
命名空间是在元素的开始标签的 xmlns 属性中定义的。
命名空间声明的语法如下。xmlns:前缀="URI"。
```
<root>

<h:table xmlns:h="http://www.spring.org/TR/html4/">
<h:tr>
<h:td>Apples</h:td>
<h:td>Bananas</h:td>
</h:tr>
</h:table>

<f:table xmlns:f="/Day1/furniture">
<f:name>African Coffee Table</f:name>
<f:width>80</f:width>
<f:length>120</f:length>
</f:table>

</root>
```

在上面的实例中，<table> 标签的 xmlns 属性定义了 h: 和 f: 前缀的合格命名空间。
当命名空间被定义在元素的开始标签中时，所有带有相同前缀的子元素都会与同一个命名空间相关联。
命名空间，可以在他们被使用的元素中或者在 XML 根元素中声明：

```
<root xmlns:h="http://www.w3.org/TR/html4/"
xmlns:f="//www.w3cschool.cn/furniture">

<h:table>
<h:tr>
<h:td>Apples</h:td>
<h:td>Bananas</h:td>
</h:tr>
</h:table>

<f:table>
<f:name>African Coffee Table</f:name>
<f:width>80</f:width>
<f:length>120</f:length>
</f:table>

</root>
```
为元素定义默认的命名空间可以让我们省去在所有的子元素中使用前缀的工作。它的语法如下
```
xmlns="namespaceURI"
```

实际使用中的命名空间
XSLT 是一种用于把 XML 文档转换为其他格式的 XML 语言，比如 HTML。
在下面的 XSLT 文档中，您可以看到，大多数的标签是 HTML 标签。
非 HTML 的标签都有前缀 xsl，并由此命名空间标识：

```
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"：

<?xml version="1.0" encoding="ISO-8859-1"?>

<xsl:stylesheet version="1.0"
xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

<xsl:template match="/">
<html>
<body>
<h2>My CD Collection</h2>
<table border="1">
<tr>
<th align="left">Title</th>
<th align="left">Artist</th>
</tr>
<xsl:for-each select="catalog/cd">
<tr>
<td><xsl:value-of select="title"/></td>
<td><xsl:value-of select="artist"/></td>
</tr>
</xsl:for-each>
</table>
</body>
</html>
</xsl:template>

</xsl:stylesheet>
```
