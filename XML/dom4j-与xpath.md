![image.png](https://upload-images.jianshu.io/upload_images/14935748-81a4fc0258c448ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####xml文件
```
<?xml version="1.0" encoding="UTF-8"?>
<person> 
  <p1 id1="1"> 
    <name>zhangshan</name>  
    <sex>male</sex> 
  </p1>  
  <p1 id2="2"> 
    <name>lisi</name> 
    <sex>fmale</sex> 
  </p1> 
</person>

```
####xpath 获取元素值
```
package xpathDemo;

import java.util.List;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;

public class Demo {

	public static void main(String[] args) throws Exception {
		
		//getXpathValue();
		getXpathValuesite();
		
	}
	//获取全部name的值

	public static void getXpathValue() throws DocumentException {
		SAXReader reader = new SAXReader();//创建解析器
		Document document = reader.read("src/person.xml");//得到解析对对象
		Element root =document.getRootElement();//获取根元素
		
		List<Node> list = root.selectNodes("//name");//得到name元素
		
		//遍历list
		
		for (Node node : list) {
			//node就是name元素
			System.out.println(node.getText());//获取元素的值
		}
	}
	//获取指未知的name下的值
	public static void getXpathValuesite() throws DocumentException {
		SAXReader reader = new SAXReader();//创建解析器
		Document document = reader.read("src/person.xml");//得到解析对对象
		Element root =document.getRootElement();//获取根元素
		
		Node node = root.selectSingleNode("//p1[@id2='2']/name");//得到name元素
		String str = node.getText();//获取内容
		System.out.println(str);
		
	
	}
	

}

```

