>描述字符串结构的接口，在这一个接口里面一般发现有三种常用的子类String类，StringBuffer类，StringBuilder类
![image.png](https://upload-images.jianshu.io/upload_images/14935748-4f04cc9427fe30e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
只要有字符串就可以为CharSequence接口实例化；
示例代码：
```
package com.study.usually;

public class Charseq {

	public static void main(String[] args) {
		CharSequence str = "www.leiyc.io";//实例化
		CharSequence charSequence = str.subSequence(0, 4);
		System.out.println(str);
		System.err.println(charSequence);
	}

}
```
输出;
www.leiyc.io
www.
和String非常的类似。



>AutoCloseable接口:主要用于日后进行资源开发处理，以实现资源自动关闭，例如：
在以后进行文件网络以及数据开发中，由于服务器资源有限，所以在使用一定要关闭资源，这样这样才可以被更多使用者所使用。
案例：
```
package com.study.usually;
interface IMessage{
	public void send ();//消息发松	
	
}
class NetMessage implements IMessage{
	private String msg;
	public  NetMessage(String msg) {
		this.msg = msg;
		
	}
	public boolean open() {
		System.out.println("【open】获取消息发送连接资源.");
		return true ;
	}
	public void close () {
		
		System.out.println("【close】关闭消息发送资源通道.");
	}
	@Override
	public void send() {
		System.out.println("【***发送消息***】" + this.msg);
	}
}

public class AutoRealse {

	public static void main(String[] args) {
		NetMessage nm =new NetMessage("www.leiyc.io");
		if(nm.open()) {//通道是否打开
			nm.send();//消息的发送
			nm.close();//关闭消息通道
		}

	}

}
```
输出结果：
【open】获取消息发送连接资源.
【***发送消息***】www.leiyc.io
【close】关闭消息发送资源通道.
AutoCloseable：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-daf5ea579175516f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/14935748-9e06180305219876.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


实现自动关闭：
```
package com.study.usually;
interface IMessage extends AutoCloseable{
	public void send ();//消息发松	
	
}
class NetMessage implements IMessage{
	private String msg;
	public  NetMessage(String msg) {
		this.msg = msg;
		
	}
	public boolean open() {
		System.out.println("【open】获取消息发送连接资源.");
		return true ;
	}
	public void close () {
		
		System.out.println("【close】关闭消息发送资源通道.");
	}
	@Override
	public void send() {
		System.out.println("【***发送消息***】" + this.msg);
	}
}

public class AutoRealse {

	public static void main(String[] args) {
		try (IMessage nm =new NetMessage("www.leiyc.io")){
			nm.send();//消息的发送
			} catch (Exception e) {
			
			}
			
		}

	}

```
AutoClosebale必须结合try - catch语句使用，方可自动关闭。换言之只能和异常捆绑使用。



>Runtime描述的是运行状态，他是单例设计模式
![image.png](https://upload-images.jianshu.io/upload_images/14935748-75f439c40cb7b51f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
开发中的应用
获取实例化对象：getRuntime();
实例：
```
package com.study.usually;

public class GetCore {

	public static void main(String[] args) {
		Runtime run = Runtime.getRuntime();//Runtime实例化对象
		System.out.println(run.availableProcessors());//获取内核数量	

	}

}
```
通过上述代码可以获取本机内核数量`


Runtime常用方法;
```
Runtime run = Runtime.getRuntime();//Runtime实例化对象
run.availableProcessors();//获取内核数量
```	

获取最大可用内存空间
```
public long maxMemory()
```
获取可用内存空间：
```
public long totalMemory()
```
获取空闲内存
```
public long freeMemory()
```
手工进行GC处理：
````
public void gc()
````

>什么是GC？如何处理？
垃圾收集器，是可有由系统调用的垃圾回收功能，也可手工调用实现回收；

