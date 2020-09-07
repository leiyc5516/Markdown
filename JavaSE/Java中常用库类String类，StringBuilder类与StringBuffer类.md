>StringBuffer
String 是一定会使用的用的工具类：
1.每一个字符串常量都属于一个String类的匿名对象，并且不可更改；
2.String有两个常量池：静态常量池，运行时常量池；
3.String类对象建议使用直接赋值的方式完成，这样可以直接将对象保存在对象池之中以方便下次使用。
由于字符串String不可修改.
StringBuffer：为了解决字符串不尅修改内容的缺点，提出StringBuffer类

>StringBuffer类必须像普通类那样去实例化，然后调用；
StringBuffer类构造方法;
public StringBuffer()
public StringBuffer​(int capacity)
public StringBuffer​(String str)
public StringBuffer​(CharSequence seq)

>StringBuffer提供了append方法追加
![image.png](https://upload-images.jianshu.io/upload_images/14935748-8c3a6adbfe8f79fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
package com.study.usually;

public class StringBufferClass {

	public static void main(String[] args) {
		
			StringBuffer buffer =new StringBuffer("hello world");
			change( buffer );
			System.out.println(buffer);//内容没发生改变
	}
	public static void change(StringBuffer temp) {
		//修改内容
		 temp .append( " baby!");
		
	}

}
```
输出：
hello world baby!

实际在编程中：
大多数情况，很少会有字符串内容的改变这种改变并不是针对于静态常量池的改变对
String strA = "www.leiyc.io";
String strB = "www."＋＂leiyc. " + "io"
strA == strB

插入![image.png](https://upload-images.jianshu.io/upload_images/14935748-b88ccf0d69e0385e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/14935748-81bc9e2b4d0a5085.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>删除指定范围的数据：
public  StringBuffer  delete​ (int start, int end)

![image.png](https://upload-images.jianshu.io/upload_images/14935748-f2c7b4540fbf8bd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>字符串反转
public StringBuffer reverse()


请解释String类，StringBuilder类与StringBuffer类区别：
String类是字符串的首选类型，其最大优点是不允许修改
StringBuffer与StringBuilder类的内容允许修改；
StringBuffer在JDK1.0出现是线程安全的，而StringBuilder是JDK1.5出现时非线程安全的类。

