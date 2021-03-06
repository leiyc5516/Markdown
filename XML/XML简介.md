>**XML，可扩展的标识语言（eXtensible Markup Language）**

>XML的主要用途
丰富文件（Rich Documents）-自定文件描述并使其更丰富；
属于文件为主的XML技术应用；
标记是用来定义一块数据应该如何呈现；
解释数据（Metadata）-描述其它文件或在线信息；
属于数据为主的XML技术应用；
标记是用来说明一块资料的意义；
组态档案（Configuration Files）-描述软件的组态参数。


>**突出使用**
用来传输和存储数据
大多配置文件都是用xml
***
#####XML 把数据从 HTML 分离
通过 XML，数据能够存储在独立的 XML 文件中。这样您就可以专注于使用 【**HTML**】/【**CSS**】 进行显示和布局，并确保修改底层数据不再需要对 HTML 进行任何的改变。只需要通过**JavaScript**解析**XML**文档就可以更新您的网页的数据内容。
***
#####XML 树结构
XML 文档形成了一种树结构，它从"根部"开始，然后扩展到"枝叶"。
树结构是通常被称为 XML 树，并且可以很容易地描述任何 XML 文档。
通过采用树状结构，你可以知道所有从根开始的后续的分行及支行。
***
#####xml实例,注意看下面注解
***
```
<?xml version="1.0"? encoding="utf-8"><!--version=""指定版本，encoding=""指定编码格式-->
<note><!--根标签-->
<to>Tove</to><!--子标签，通常跟存储数据相关信息来指定标签的字符-->
<from>Jani</from><!--元素都必须有关闭标签-->
<heading>Reminder</heading><!--XML 标签对大小写敏感-->
<body>Don't forget me this weekend!</body>
<b><i>This text is bold and italic</i></b><!--在 XML 中，所有元素都必须彼此正确地嵌套-->
</note>
```
****
####实体引用
在 XML 中，一些字符拥有特殊的意义。如果您把字符 "<" 放在 XML 元素中，会发生错误，这是因为解析器会把它当作新元素的开始。
在 XML 中，有 5 个预定义的实体引用：
```
&lt;      <      less than
&gt;       >      greater than
&amp;      &      ampersand
&apos;      '      apostrophe
&quot;      "      quotation mark
```
注释：在 XML 中，只有字符 "<" 和 "&" 确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯。
***
###XML 属性值必须加引号
与 HTML 类似，XML 元素也可拥有属性（名称/值的对）。
在 XML 中，XML 的属性值必须加引号。
一个元素可以有多个独特的属性。属性提供了有关XML元素的详细信息 XML属性始终是一个名称 值对

>避免 XML 属性？
因使用属性而引起的一些问题：
属性不能包含多个值（元素可以）
属性不能包含树结构（元素可以）
属性不容易扩展（为未来的变化）
属性难以阅读和维护。请尽量使用元素来描述数据。而仅仅使用属性来提供与数据无关的信息。
***

XML 文档必须有一个根元素
XML元素都必须有一个关闭标签
XML 标签对大小写敏感
XML 元素必须被正确的嵌套
XML 属性值必须加引号
