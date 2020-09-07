>Thread类实现多线程

>多线程主要实现的方式：
>第一类实现：Thread类实现
>Thread类包含在java.lang包中是一个基本的类，继承至Object类，该类可以包含内所有的一切性质，但是需要多线程执行的程序方法，必须放到**run()**中执行。并且在主方法中任何的一个Thread线程对象都必须用**start()**启动，而start方法本身是没有实现的他主要是调用start0，而虚拟机中start0代为何操作系统协商调用操作系统的多线程方法，从而实现多线程调用。
下面是一个线程类的定义实现,。
```
class MyThread extends Thread{
	private String title;
	public MyThread( String title) {
		this.title = title;
		// TODO Auto-generated constructor stub
	}
	@Override
	public void run() {
			// TODO Auto-generated method stub
		super.run();
		for (int i = 0; i < 10; i++) {
			System.out.println("title" + this.title + "执行第" + i +"次");	
		}
	}
	//线程的主体类
}
public class ThreadDemo {
	public static void main( String [] args) {

		new MyThread("线程A").start();//产生一个线程执行start方法，调用后线程内部的执行,交由系统处理                                      
                new MyThread("线程B").start();  
		new MyThread("线程C").start();
                MyThread mt  = new MyThread("线程D");
/****
                mt.start();
                mt.start();
                多次启动同一个线程会产生异常
****/
	}

}
```
>**题外话：**
>JNI是Java Native Interface的缩写，通过使用 [Java](https://baike.baidu.com/item/Java/85979)本地接口书写程序，可以确保代码在不同的平台上方便移植。<sup> [1]</sup>  从Java1.1开始，JNI标准成为java平台的一部分，它允许Java代码和其他语言写的代码进行交互。JNI一开始是为了本地已[编译](https://baike.baidu.com/item/%E7%BC%96%E8%AF%91/1258343)语言，尤其是C和C++而设计的，但是它并不妨碍你使用其他编程语言，只要调用约定受支持就可以了。使用java与本地已编译的代码[交互](https://baike.baidu.com/item/%E4%BA%A4%E4%BA%92/6964417)，通常会丧失平台[可移植性](https://baike.baidu.com/item/%E5%8F%AF%E7%A7%BB%E6%A4%8D%E6%80%A7/6931884)。但是，有些情况下这样做是可以接受的，甚至是必须的。例如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序的性能。JNI标准至少要保证[本地代码](https://baike.baidu.com/item/%E6%9C%AC%E5%9C%B0%E4%BB%A3%E7%A0%81)能工作在任何Java [虚拟机](https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E6%9C%BA)环境。
保证JVM可以顺利调用操作系统的算法。


>多线程第二类实现方式：
>Runnable接口的产生是为了破除Java中单继承所带来的不变；他是多线程实现的另外一种方式。JDK1.8以后Lambda的Runnable接口变成了函数式的接口
案例：
```
class My2Thread implements Runnable{
	private String title;
	public My2Thread( String title) {
		this.title = title;
		// TODO Auto-generated constructor stub
	}
	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println("title" + this.title + "执行第" + i +"次");
			
		}// TODO Auto-generated method stub
		
	}
}
//实现多线程Runnable接口的类的对象可以作为参数传递给Thread类对象，再通过Thread调用start方法，启动多线程
public class ThreadDemo {
	public static void main( String [] args) {
		My2Thread mt = new My2Thread("线程1");
		My2Thread mt1 = new My2Thread("线程2");
		My2Thread mt2= new My2Thread("线程3");
		new Thread(mt).start();
		new Thread(mt1).start();
		new Thread(mt2).start();
	}

}
```
>优先考虑使用Runnable接口实现多线程；并且无论如何都是Thread类对象调用start方法完成多线程的启动。

>Thread类和Runnable接口：
-Thread其实是继承Object类实现Runnable接口的子类
-**start()**调用Thread 中**run()**方法，此**run()**调用Runnable子类被覆写的中**run()**方法。
![Thread.png](https://upload-images.jianshu.io/upload_images/14935748-cb0aa808f85c3d91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>用Runnable描述资源，Thread描述线程。
![FW.png](https://upload-images.jianshu.io/upload_images/14935748-970459d62a97f704.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>target（Runnable接口子类对象）用来描述共享资源，Thread用来描述多个线程，虽然Thread子类本身也能描述资源但是在利用另外的Thread子类的start方法来启动，不利于程序开发安全性。
![内存分析图.png](https://upload-images.jianshu.io/upload_images/14935748-14d9fdef37db15fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
案例;
```
package com.mybaits.test;

class SellTicket implements Runnable{
	private int ticket = 5;
	@Override
	public void run() {
		if (ticket>0) {
			System.out.println("卖出第"+ (ticket--) +"张票");
		}
		// TODO Auto-generated method stub
		
	}
	
}

public class Ticket {
	public static void main(String [] args) {
		SellTicket tc = new SellTicket();//线程共享资源
		new Thread(tc).start();//第一个线程
		new Thread(tc).start();//第二个线程
		new Thread(tc).start();//第三个线程
		
	}

}
/****运行结果：
卖出第5张票
卖出第4张票
卖出第5张票

卖出第5张票
卖出第3张票
卖出第4张票

卖出第4张票
卖出第3张票
卖出第5张票


*****/
```

>多线程的第三种实现方式Callable接口：由于run()方法没有返回值，满足不了某些开发需求!
java.util.concurrent.Callable
Callable与Thread关联关系：
![relation.png](https://upload-images.jianshu.io/upload_images/14935748-7b2d2641672de2e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
案例：
```
package com.mybaits.test;
import java.util.concurrent.*;
class My3Thread implements Callable<String >{

	@Override
	public String  call() throws Exception {
		for (int i = 0; i < 10; i++) {
			System.out.println("********线程执行输出数据x=" + i);
		}
		// TODO Auto-generated method stub
		return "线程执行完毕";
	}	
}

public class CallableDemo {
	public static void main(String []args) throws InterruptedException, ExecutionException {
		FutureTask<String> task = new FutureTask<>(new My3Thread());
		new Thread(task).start();
		System.out.println("【线程执行返回】"+task.get());	
	
	}

}
/****
输出：
********线程执行输出数据x=0
********线程执行输出数据x=1
********线程执行输出数据x=2
********线程执行输出数据x=3
********线程执行输出数据x=4
********线程执行输出数据x=5
********线程执行输出数据x=6
********线程执行输出数据x=7
********线程执行输出数据x=8
********线程执行输出数据x=9
【线程执行返回】线程执行完毕
***/

```
>Runnable与Callable两者的区别：
-RunnableJDK1.0以后提出的实现多线程方法，CallableJDk1.5提出的实现多线程方法；
- java.lang.Runnable; 只是提供了run()方法并且没有返回值；
-java.util.concurrent.Callable;提供了call（）方法可以存在返回值。

>对上面三中实现方法：不变的是启动多线程必须使用Thread对象调用start（）方法才能实现。





