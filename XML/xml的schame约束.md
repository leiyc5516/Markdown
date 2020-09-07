1.schema本身也是xml文件 ，所以本身也要遵循xml的约束
2.schema文件的后缀名.xsd
![image.png](https://upload-images.jianshu.io/upload_images/14935748-259596c8ae8d0214.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-f48e1712aa506cc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-8aa467d653cee6a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-76d59605325471d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####被约束的xml文件
```
<?xmlversion="1.0" encoding="UTF-8"?>
<note
xmlns="http://www.w3school.com.cn" #默认命名空间的声明，在此xml文档中使用过的所有元素都被声明于该命名空间。
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" #实例命名空间，只有有了xsi之后才能使用schemaLocation，该值提供了需要使用的命名空间
xsi:schemaLocation="http://www.w3school.com.cn note.xsd">   #提供命名空间使用的xml schema的位置
<to>CZ</to>
<from>Jason</from>
<heading>Greeting</heading>
<body>Hello CZ!</body>
</note>
```
####约束文件
```
<?xmlversion="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" #表明schema中用到的元素和数据类型来自该URI的命名空间，而且规定来自该命名空间的元素和数据类型应使用前缀xs
targetNamespace="http://www.w3school.com.cn"    #表明此schema定义的元素来自该命名空间
xmlns="http://www.w3school.com.cn" #默认命名空间
elementFormDefault="qualified"> #任何xml实例文档所使用的且在此schema中声明过的元素必须被命名空间限定
    <xs:element name="note">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="to" type="xs:string"/>
                <xs:element name="from" type="xs:string"/>
                <xs:element name="heading" type="xs:string"/>
                <xs:element name="body" type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```
笔记参考及补充
[简单元素](https://blog.csdn.net/CristianoJason/article/details/51282739)
[复杂元素](https://blog.csdn.net/CristianoJason/article/details/51327041)

