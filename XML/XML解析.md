###xml一共有两种解析的方式dom和sax解析技术
![xml两种解析方式](https://upload-images.jianshu.io/upload_images/14935748-742ca2ed5d6436d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####针对上面的技术不同公司和不同的组织通过api提供不同的解析器：
sun公司提供了 jaxp 在Javase可以查看JDK自带，在javax.xml.parsers包内部
**domj4组织提供  domj4**
jdom组织提供  jdom

```
<?xml version="1.0" encoding="UTF-8"?>
<person>
	<p1>
		<name>zhangshan</name>
		
	</p1>
	<p1>
		<name>lisi</name>
		
	</p1>

</person>
```

####通过jaxp实现查询节点
```
package src;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class Test {

	public static void main(String[] args) throws Exception {
		selectAll();
		System.out.println();
		selectSingle();

	}
	//查询xml某一元素的第一个值
	public static void selectSingle() throws Exception {
		/*
		 * 1.创建解析工厂
		 * 2.根据解析工厂创建解析器
		 * 3.根据解析xml返回doucment对象
		 * 4.得到所有的元素name
		 * 5.返回集合里面的item，下标获取具体元素
		 * 6.getTextContent();//得到标签内容
		 */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
		Document document = builder.parse("src/person.xml");//得到doc对象
		String string = document.getElementsByTagName("name").item(1).getTextContent();
		System.out.println(string);
		
	}
	private static void selectAll() throws Exception {
		// 查询所有name值
		/*
		 * 1.创建解析工厂
		 * 2.根据解析工厂创建解析器
		 * 3.根据解析xml返回doucment对象
		 * 4.得到元素
		 * 5.得到标签内容
		 */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		try {
			DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
			Document document = builder.parse("src/person.xml");//得到doc对象
			NodeList list = document.getElementsByTagName("name");//得到元素为name的所有的集合
			for (int i = 0; i < list.getLength(); i++) {
				Node nameNode = list.item(i);//得到每一个name 元素
				String str = nameNode.getTextContent();//得到标签内容
				System.out.println(str);
			}
		} catch (ParserConfigurationException e) {
			throw e;
		}
	}

}

```

#####使用jaxp添加节点
![image.png](https://upload-images.jianshu.io/upload_images/14935748-ea2bd708d4044066.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
public static void addSex() throws Exception {
		 /* 
		  * 1.创建解析工厂
		  * 2.根据解析工厂创建解析器
		  * 3.根据解析xml返回doucment对象
		  * 4.得到要创建元素相对父节点，使用item注明要创建的位置
		  * 5.createElement
		  * 6.createTextNode
		  * 7.appendChild
		  * 8.把创建好的放在指定位置
		  * 
		  * 9.回写xml
		  */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
		Document document = builder.parse("src/person.xml");//得到doc对象
		Node p1 = document.getElementsByTagName("p1").item(0);
		Element sex = document.createElement("sex");//创建标签
		Text male = document.createTextNode("male");//创建文本值
		sex.appendChild(male);//把文本添加到标签上
		p1.appendChild(sex);//把sex添加到p1下
		/*
		 * 回写xml
		 */
		TransformerFactory factory = TransformerFactory.newInstance();
		Transformer transformer = factory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));
	}
	

```


####使用修改节点
```
public static void modifySex() throws Exception {
		 /* 
		  * 1.创建解析工厂
		  * 2.根据解析工厂创建解析器
		  * 3.根据解析xml返回doucment对象
		  * 4.使用item注明要修改的位置
		  * 5.setTextContext修改内容
		  * 6.回写xml
		  */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
		Document document = builder.parse("src/person.xml");//得到doc对象
		Node sex = document.getElementsByTagName("sex").item(0);
		sex.setTextContent("female");
		TransformerFactory factory = TransformerFactory.newInstance();
		Transformer transformer = factory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));
	}
```

####使用删除节点
```
public static void delEle() throws  Exception {
		 /* 
		  * 1.创建解析工厂
		  * 2.根据解析工厂创建解析器
		  * 3.根据解析xml返回doucment对象
		  * 4.使用item注明要删除的位置，获取父节点，再指定
		  * 5.removeChild删除内容
		  * 6.回写xml
		  */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
		Document document = builder.parse("src/person.xml");//得到doc对象
		Node sex  = document.getElementsByTagName("sex").item(0);
		Node p1 = sex.getParentNode();
		p1.removeChild(sex);
		TransformerFactory factory = TransformerFactory.newInstance();
		Transformer transformer = factory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/person.xml"));
	}
	
```
####遍历元素
```
	/*
	 * 遍历元素
	 */
	public static void listAll() throws Exception {
		 /* 
		  * 1.创建解析工厂
		  * 2.根据解析工厂创建解析器
		  * 3.根据解析xml返回doucment对象
		  * ======递归实现遍历=======
		  * 4.得到根节点递归其子节点
		  */
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();//创建解析工厂
		DocumentBuilder builder = builderFactory.newDocumentBuilder();//创建解析器
		Document document = builder.parse("src/person.xml");//得到doc对象
		list(document);//递归遍历
	}
	
	private static void list(Node node) {
		if (node.getNodeType()==Node.ELEMENT_NODE) {//判断是否是元素
			System.out.println(node.getNodeName());//打印名称
		}
		NodeList nodeList = node.getChildNodes();//获取该节点下所有的子节点
		for (int i = 0; i < nodeList.getLength(); i++) {
			list(nodeList.item(i));//递归子节点
		}
	}
```
