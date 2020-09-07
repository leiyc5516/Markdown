>[摘抄1](https://www.cnblogs.com/zyxsblogs/p/10108706.html)
[摘抄2](https://www.jianshu.com/p/3b4707c13c39)

![image.png](https://upload-images.jianshu.io/upload_images/14935748-18512b83777d76ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<?xml version="1.0" encoding="UTF-8"?>
<student id="001">
  <name>张三</name>
  <sex>男</sex>
  <age>20</age>
</student>
```
```
package com.java1234.xml;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class SAX01 extends DefaultHandler{

    @Override
    public void startDocument() throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
    }

    @Override
    public void endDocument() throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("文档加载完毕！");
    }

    @Override
    public void startElement(String uri, String localName, String qName, 
            Attributes attributes) throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("元素节点开始加载"+qName);
    }

    @Override
    public void endElement(String uri, String localName,
            String qName) throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("元素节点结束加载"+qName);
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("扫描文本节点"+new String(ch,start,length));
    }
    
    public static void main(String[] args) throws Exception {
        SAXParserFactory factory=SAXParserFactory.newInstance();
        SAXParser parser=factory.newSAXParser();
        parser.parse("src/demo01.xml", new SAX01());
    }
}
```

```
package com.java1234.model;

public class Student {
    private String name;
    private String sex;
    private String id;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        // TODO Auto-generated method stub
        return this.id+","+this.name+","+this.age+","+this.sex;//重写toString 方法
    }
    
}
```
```package com.java1234.model;

public class Student {
    private String name;
    private String sex;
    private String id;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        // TODO Auto-generated method stub
        return this.id+","+this.name+","+this.age+","+this.sex;//重写toString 方法
    }
    
}
```
```
package com.java1234.xml;

import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

import com.java1234.model.Student;

public class SAX3 extends DefaultHandler{
    private List<Student>students=null;
    private Student student=null;
    private String tag=null;//标记上一个节点
    
    @Override
    public void startDocument() throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("开始读取学生信息");
        students=new ArrayList<Student>();
    }

    @Override
    public void endDocument() throws SAXException {
        // TODO Auto-generated method stub
        System.out.println("\n 学生信息读取完毕");
    }

    @Override
    public void startElement(String uri, String localName, String qName, 
            Attributes attributes) throws SAXException {
        // TODO Auto-generated method stub
        //attributes 属性节点操作
        if("student".equals(qName)){
            student=new Student();
            student.setId(attributes.getValue(0));
        }
        tag=qName;
    }

    @Override
    public void endElement(String uri, String localName,
            String qName) throws SAXException {
        // TODO Auto-generated method stub
        if("student".equals(qName)){
            students.add(student);
            student=null;
        }
        tag=null;
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        // TODO Auto-generated method stub
        if(tag!=null){
            String content=new String(ch,start,length);
            if("name".equals(tag)){
                student.setName(content);
            }else if("age".equals(tag)){
                student.setAge(Integer.parseInt(content));
            }else if("sex".equals(tag)){
                student.setSex(content);
            }
        }
    }
    
    public static void main(String[] args) throws Exception {
        SAXParserFactory factory=SAXParserFactory.newInstance();
        SAXParser parser=factory.newSAXParser();
        SAX3 sax3=new SAX3();
        parser.parse("src/students.xml", sax3);
        for(Student s:sax3.students){
            System.out.println(s);
        }
    }
}
```

DOM解析xml文档原理：一次性将xml文档加载进内存，然后再内存中构建Document树。 

DOM解析：不适合读取大容量的文件，容易导致内存溢出。

SAX解析原理：加载一点，读取一点，存储一点。对内存要求比较低。

SAX解析工具：Sun公司提供的（内置在jdk中）

SAX核心API：SAXParser类：用于读取和解析API文档的类

            parse（File f,DefaultHandler dh）方法：解析xml文件

            参数一：File:表示读取的xml文件

            参数二：DefaultHandler：SAX处理程序的父类   使用DefaultHandler的子类

DefaultHandler类的API：
```
void startDocument（）：在读到文档开始时调用

void endDocument（）：在读到文档结束时调用

 void startElement(Stringuri,StringlocalName,StringqName,Attributesattributes):在读到元素开始时调用

 void  endElement(Stringuri,StringlocalName,StringqName)：读到结束标签时调用

void characters(char[] ch, int start, int length)   读取文本的时候调用
```
>[详解](https://blog.csdn.net/ydxlt/article/details/50183693)




