![image.png](https://upload-images.jianshu.io/upload_images/14935748-e24dda0ad20d5b96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-da395105b1ea8b0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####XML文件
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

####查询元素
```
	/*
	 * 获取元素值
	 */
	public static void selectName() throws Exception {
		/*
		 * 1.创建解析器
		 * 2.得到Document对象
		 * 3.得到根节点
		 * 
		 * 4.得到p1下所有元素集合
		 * 5.得到旗下的name
		 * 6.得到name下面的值
		 */
		SAXReader saxReader = new SAXReader();//创建解析器
		Document document = saxReader.read("src/person.xml");//得到Document对象
		Element root =document.getRootElement();//得到根节点
		List<Element> list = root.elements("p1");//得到p1下所有元素集合
        Element p2 = list.get(1);//获取第二个p
		System.err.println(p2.element("name").getText());//获取第二个p下面的name
		for (Element element : list) {//遍历p1
			//element是每个p1的元素
			//得到element下面name
			Element name = element.element("name");//得到单个元素
			System.out.println(name.getText());//输出获取的值
		}
	}
```


####使用dom4j实现在末尾添加节点的操作

```
/*
	 * 添加元素
	 */
	public static void addSex() throws Exception {
		/*
		 * 1.创建解析器
		 * 2.得到Document对象
		 * 3.得到根节点
		 * 
		 * 4.得到p1
		 * 5.在p1下添加元素
		 * 6.添加文本
		 * 
		 * 7.回写xml
		 */
		
		SAXReader saxReader = new SAXReader();//创建解析器
		Document document = saxReader.read("src/person.xml");//获得
		Element root = document.getRootElement();//获取到根元素
		List< Element> p = root.elements("p1");//获取p1所有元素
		Element p1 = p.get(0);//获取p1元素的第一
		Element sex= p1.addElement("sex");//添加元素
		sex.addText("male");//添加元素内容
		
		//回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//创建format格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		
	}
```
####使用dom4j实现在指定位置添加节点的操作
```
public static void addage() throws Exception {
		/*
		 * 1.创建解析器
		 * 2.得到Document对象
		 * 3.得到根节点
		 * 
		 * 4.得到p1
		 * 5.在p1特定位置下添加元素
		 * 6.增加文本
		 * 
		 * 7.回写xml
		 */
		
		SAXReader saxReader = new SAXReader();//创建解析器
		Document document = saxReader.read("src/person.xml");//获得
		Element root = document.getRootElement();//获取到根元素
		Element p = root.element("p1");
		List< Element> p1 = p.elements();//获取p1所有元素
		Element age =  DocumentHelper.createElement("age");//创建元素
		age.addText("20");//给元素添加文本
		p1.add(2, age);
		
		//回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//创建format格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
		
	}
```

####使用dom4j实现修改节点的操作
```
	/*
	 * 修改元素
	 */
	public static void changeEle() throws Exception {
		/*
		 * 1.创建解析器
		 * 2.得到Document对象
		 * 3.得到根节点
		 * 
		 * 4.得到p1
		 * 5.获取元素
		 * 6.修改元素的text
		 * 
		 * 7.回写xml
		 */
		
		SAXReader saxReader = new SAXReader();//创建解析器
		Document document = saxReader.read("src/person.xml");//获得
		Element root = document.getRootElement();//获取到根元素
		Element p = root.element("p1");//获取p1标签
		Element age = p.element("age");//获取age所有元素
		age.setText("25");//修改age的数据
		//回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//创建format格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
		
	}
```

####使用dom4j实现删除节点的操作
```
/*
	 * 删除元素
	 */
	public static void deleteEle() throws Exception {
		/*
		 * 1.创建解析器
		 * 2.得到Document对象
		 * 3.得到根节点
		 * 
		 * 4.得到p1
		 * 5.在p1特定位置下添加元素
		 * 6.
		 * 
		 * 7.回写xml
		 */
		
		SAXReader saxReader = new SAXReader();//创建解析器
		Document document = saxReader.read("src/person.xml");//获得
		Element root = document.getRootElement();//获取到根元素
		Element p = root.element("p1");//获取p1标签
		Element age = p.element("age");//获取age所有元素
		p.remove(age);//移除节点
		/*
		 * 也可以获取父节点再利用父节点删除
		 */
                System.out.println(p.attributeValue("id1"));//获取属性值
		OutputFormat format = OutputFormat.createPrettyPrint();//创建format格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
		
	}
```
