>I/O流：输入输出实际上是byte流的一种传递。输入输出是相对的，并不是绝对的。
![io.png](https://upload-images.jianshu.io/upload_images/14935748-119d7de6990016b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

流的主要处理方式：
-字节处理流：OutputStream（输出字节流），InputStream（输出字节流）；
-字符处理流：Writer（输出字符流）， Reader（输入字符流）。
明确Java中所有操作都是以类来完成：
![ioClass.png](https://upload-images.jianshu.io/upload_images/14935748-d78989a2df7d4b2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

流的操作步骤：
1.通过字节流或者字符流的子类为父类对象实例化
2.利用字节流或者字符流中的方法实现数据的输入输出操作
3.流的操作是对资源的操作，资源必须关闭处理

利用文件操作，做案例：

**OutputStream字节输出流**:   定义的公共输出标准，一共包含三个输出方法，使用的时候要向上转型实现：
```
OutputStream output =new FileOutputStream(file,true);//子类实例化,true 表示追加
```
1.public abstract void write​(int b) throws IOException  输出单个字节
2.public void write​(byte[] b) throws IOException 输出一组字节数据   **常用**
3.public void write​(byte[] b,  int off,  int len)  throws  IOException 输出部分字节数据  **常用**

![out_relation.png](https://upload-images.jianshu.io/upload_images/14935748-2e0a84edd28a0c8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![upType.png](https://upload-images.jianshu.io/upload_images/14935748-157123cedbeb1088.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

案例1：
```
package com.study.threeday.io;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class FileIODemo {

	public static void main(String[] args) throws IOException {
		File file= new File("F:"+File.separator+"test"+File.separator+"mldn.txt");
		if(!file.getParentFile().exists()) {//1.指定操作文件路径
			file.getParentFile().mkdirs();	
		}
//		if(file.createNewFile())
//			System.out.println("【创建文件】");
		//执行下面操作系统自动创建文件
		OutputStream output =new FileOutputStream(file,true);//2.子类实例化,true 表示追加
		String str = "www.mldn.com";//输出想要的内容
		output.write(str.getBytes());//3.将字符转变成字节数组
		output.close();//4.关闭资源

	}

}
```

**InputStream字节输入流**: 
1.public abstract int read​() throws IOException读取单个直接方法；读取到低返回-1
2.public int read​(byte[] b) throws IOException读取一组字节方法； **常用**  返回的读取的长度
3.public int read​(byte[] b, int off,int len) throws IOException读取部分字节方法
4.JDK1.9之后：public byte[] readAllBytes​() throws IOException 不推荐使用

![iClass.png](https://upload-images.jianshu.io/upload_images/14935748-7c9b58a94ea12091.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

案例：
```
package com.study.threeday.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

public class FileIODemo2 {

	public static void main(String[] args) throws IOException {
		File file= new File("F:"+File.separator+"test"+File.separator+"mldn.txt");
		InputStream  input = new FileInputStream(file);
		byte data[] =  new byte [1024];//开辟数组缓存保存数据
		int len=input.read(data);//读取数据保存到字节数组之中
		System.out.println("【"+new String(data,0,len)+"】");
		input.close();
		// TODO Auto-generated method stub

	}

}
```

>**Writer**:
![writer.png](https://upload-images.jianshu.io/upload_images/14935748-7ef168aeb609d905.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输出方法：
-输出字符数组：public void write​(char[] cbuf)  throws IOException
-输出字符串：public void write​(String str) throws IOException
案例：
```
package com.study.threeday.io;

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.Writer;

public class FileIODemo3 {
	public static void main(String[] args) throws IOException {
		File file= new File("F:"+File.separator+"test"+File.separator+"mldn.txt");
		if(!file.getParentFile().exists()) {//1.指定操作文件路径
			file.getParentFile().mkdirs();	
		}
		Writer out = new FileWriter(file,true);
		String str = "www.leiyc.io";
		out.write(str);
		out.append("中国人民万岁");
		out.close();
	}

}
```

>Reader:
![Reader.png](https://upload-images.jianshu.io/upload_images/14935748-35ea1b6f3a1f77b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入方法：只有利用字符数组完成没有字符串：
1.public int read​() throws IOException
2.public int read​(char[] cbuf) throws IOException
3.public abstract int read​(char[] cbuf,int off,int len) throws IOException
案例：
```
package com.study.threeday.io;

import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class FileIODemo4 {

	public static void main(String[] args) throws IOException {
		File file= new File("F:"+File.separator+"test"+File.separator+"mldn.txt");
		if (file.exists()) {
			Reader in = new FileReader(file);
			char data[] = new char[1024];
			int len =in.read(data);
			System.out.println("读取内容  "+new String(data,0,len));
			in.close();
			
		}
		// TODO Auto-generated method stub

	}

}
```

>字节流与字符流的区别：
**Writer**如果没使用close()方法完成关闭，那就不会输出内容到文件，close有强制刷新。可以使用flush强制，输入文件。
字节流没有缓存区，字符流有缓存区，字符流适合中文输出。


>转换流：通过向上转型将字节流对象转换为字符流对象
实现不同流之间的转换
InputStreamReader：构造方法：public InputStreamReader​(InputStream in)
OutputStreamWriter：构造方法: public OutputStreamWriter​(OutputStream out)
![Exchange.png](https://upload-images.jianshu.io/upload_images/14935748-ffce415a4022599b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>**继承关系**：
![FileWriter.png](https://upload-images.jianshu.io/upload_images/14935748-92ce47b242426b80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![FileReader.png](https://upload-images.jianshu.io/upload_images/14935748-28af519e6a733dc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>计算机中数据传输流程：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-a0dca3dc4a4b110f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




