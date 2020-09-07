>System 类：
数组拷贝：public static void arraycopy​(Object src,int srcPos,Object dest,int destPos,int length)
获取当前系统时间数值long：public static long currentTimeMillis()
垃圾回收：public static void gc()
范例：
垃圾费时：
![image.png](https://upload-images.jianshu.io/upload_images/14935748-e43c6d9ac880161c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[@Deprecated](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Deprecated.html "annotation in java.lang")([since](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Deprecated.html#since())="9") protected void finalize() throws [Throwable](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Throwable.html "class in java.lang")


>对象克隆：
使用Object中的clone方法，使用已有的对象创建一个新的对象。
protected [Object](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/Object.html "class in java.lang") clone() throws [CloneNotSupportedException](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/lang/CloneNotSupportedException.html "class in java.lang")
被克隆的对象必须接受：
```
package com.study.usually;
class Member implements Cloneable{
	private String name ;
	private int age ;
	 public Member(String name, int age){
		 this.name = name;
		 this.age = age;
		 
	 }
	@Override
	public String toString() {
		
		return "【"+super.toString()+"】name" + this.name + this.age;
	}
	
	@Override
	protected Object clone() throws  CloneNotSupportedException {
		return super.clone();//调用父类clone方法
		
	}
}

public class ImplClone {

	public static void main(String[] args) throws CloneNotSupportedException {
	Member A = new Member("领钱", 30);
	Member B = (Member) A.clone();
	System.err.println(A);
	System.out.println(B);

	}

}
```
输出：
【com.study.usually.Member@2ff4acd0】name领钱30
【com.study.usually.Member@54bedef2】name领钱30

Cloneable接口是一种能力标示接口；无任何方法，一定被实现调用，就会被认为可以接受clone。
