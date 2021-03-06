静态代理模式：

```java
package cn.leiyc.StaticProxy;

public interface Rent {
	public void rent();//接口定义行为,作为一种联系保证了代理的类的活动与被代理类的活动关系一致性
}

```

```java 
public class Service implements Rent{//被代理类实现Rent接口是为了保证中介proxy和service一致
	public void rent() {
		System.out.println("房屋出租");
	}

}

```

```java
public class Proxy implements Rent {//代理类，实现Rent接口是为了保证中介proxy和service一致
	private Service service;
	public Proxy(){//无参构造
		
	}
	public void setService(Service service) {//set方法设置属性值
		this.service = service;
	}
	public Proxy(Service service) {//有参构造设置属性值
		this.service = service;
	}
	//租房
	public void rent() {//保证代理方法功能的一致性,同时可以增强这个被代理的类的需要增强的方法
		seeHouse();
		service.rent();
		fare();
	}
	//看房
	private void seeHouse() {
		System.out.println("带房客看房");
		
	}
	//收中介费
	private void fare() {
		System.out.println("收取中介费");
		
	}
}


```

```java
import org.junit.jupiter.api.Test;

public class Client {
	private Service service;
	private Proxy proxy;
	@Test
	public void getRoom() {//客服端实现业务请求
		service = new Service();
		proxy = new Proxy( service);
		proxy.rent();//通过代理类实现rent业务
		
		
	}

}

```

> **静态代理角色分析：**
>
> > 抽象角色：一般是接口或者是抽象类，接口或者抽象类中存放需要代理的业务实现方法；
> >
> > 真实角色：被代理的角色
> >
> > 代理角色：代理真实角色，一般会对代理角色去增强代理的业务实现方法
> >
> > 客户：使用代理角色来完成业务
>
> **使用静态代理的好处：**
>
> > 使得真实角色业务更加纯粹，不在关注公共的事情，把一些公共的事情放在代理类中添加。
> >
> > 公共业务代理完成，实现业务分工。
> >
> > 公共业务扩展是只需要修改代理，使得更加集中化 
>
> **使用静态代理的缺点：**
>
> > 类多了----代理类多了，工作量变大。开发效率降低。

> **动态代理**：
>
> 动态代理和静态代理的角色是一样的;
>
> >抽象角色：一般是接口或者是抽象类，接口或者抽象类中存放需要代理的业务实现方法；
>
> > 真实角色：被代理的角色
>
> > 代理角色：代理真实角色，一般会对代理角色去增强代理的业务实现方法
>
> > 客户：使用代理角色来完成业务
>
> 动态代理类是动态生成的，静态代理的 类是写好的。
>
> **动态代理大致分为两类**
>
> > 一类是基于接口的动态代理和基于类的动态代理：
> >
> > > 基于接口的动态代理是基于JDK的动态代理；
> > >
> > > 基于类的动态代理是cglib和Javasist生成动态代理。
>
> JDK的动态代理：
>
> > java动态代理机制中有两个重要的类和接口
> >
> > InvocationHandler（接口）：
> >
> > > 是代理实例  调用处理程序  实现的接口 每个代理程序都是有一个关联的调用处理程序，对代理程序调用方法是将调用方法惊醒编码并指派到他的调用程序的invoke方法。
> >
> > ```java 
> >     /**
> >     * proxy:代理类代理的真实代理对象com.sun.proxy.$Proxy0
> >     * method:我们所要调用某个对象真实的方法的Method对象
> >     * args:指代代理对象方法传递的参数
> >     */
> >     public Object invoke(Object proxy, Method method, Object[] args)
> >         throws Throwable;
> > ```
> >
> > 
> >
> > Proxy（类）
> >
> > > Proxy类就是用来创建一个代理对象的类，它提供了很多方法，但是我们最常用的是newProxyInstance方法。
> >
> > ```java
> >     public static Object newProxyInstance(ClassLoader loader, 
> >                                             Class<?>[] interfaces, 
> >                                             InvocationHandler h)
> > ```
> >
> > ```java 
> >      Returns an instance of a proxy class for the specified interfaces
> >      that dispatches method invocations to the specified invocation
> >      handler.  This method is equivalent to:
> > 
> > /*
> > *
> > 这个方法的作用就是创建一个代理类对象，它接收三个参数，我们来看下几个参数的含义：
> > loader：一个classloader对象，定义了由哪个classloader对象对生成的代理类进行加载
> > interfaces：一个interface对象数组，表示我们将要给我们的代理对象提供一组什么样的接口，如果我们提供了这样一个接口对象数组，那么也就是声明了代理类实现了这些接口，代理类就可以调用接口中声明的所有方法。
> > h：一个InvocationHandler对象，表示的是当动态代理对象调用方法的时候会关联到哪一个InvocationHandler对象上，并最终由其调用。
> > *
> > */
> > 
> > ```
> >
> > 案例 ：
> >
> > ```java
> > package cn.leiyc.DynamicProxy;
> > 
> > public interface Rent {
> > 	public void rent();//接口定义行为
> > }
> > 
> > ```
> >
> > ```Java
> > package cn.leiyc.DynamicProxy;
> > 
> > public class Service implements Rent{//实现Rent接口是为了保证中介proxy和service一致
> > 	public void rent() {
> > 		System.out.println("房屋出租");
> > 	}
> > 
> > }
> > 
> > ```
> >
> > ```java
> > package cn.leiyc.DynamicProxy;
> > 
> > import java.lang.reflect.InvocationHandler;
> > import java.lang.reflect.Method;
> > import java.lang.reflect.Proxy;
> > 
> > public class ProxyInvocationHandler  implements InvocationHandler{
> > 	private Rent Rent;//代理的方法对象实体
> > 	public void setRent(Rent rent) {
> > 		Rent = rent;
> > 	}
> > 	/*
> > 	*
> > 	这个方法的作用就是创建一个代理类对象，它接收三个参数，我们来看下几个参数的含义：
> > 	loader：一个classloader对象，定义了由哪个classloader对象对生成的代理类进行加载
> > 	interfaces：一个interface对象数组，表示我们将要给我们的代理对象提供一组什么样的接口，如果我们提供了这样一个接口对象数组，那么也就是声明了代理类实现了这些接口，代理类就可以调用接口中声明的所有方法。
> > 	h：一个InvocationHandler对象，表示的是当动态代理对象调用方法的时候会关联到哪一个InvocationHandler对象上，并最终由其调用。
> > 	*
> > 	*/
> > 	public Object getProxy() {//生成代理对象
> > 		return Proxy.newProxyInstance(this.getClass().getClassLoader(),Rent.getClass().getInterfaces(), this);
> > 		
> > 	}
> > 	
> > 	
> > 	
> > 	
> > 		/*
> > 	    * proxy:代理类代理的真实代理对象com.sun.proxy.$Proxy0
> > 	    * method:我们所要调用某个对象真实的方法的Method对象
> > 	    * args:指代代理对象方法传递的参数
> > 	    */
> > 	@Override
> > 	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
> > 		seeHouse();
> > 		Object result = method.invoke(Rent, args);
> > 		fare();
> > 		return result;
> > 	}
> > 	//看房
> > 	private void seeHouse() {
> > 		System.out.println("带房客看房");
> > 		
> > 	}
> > 	//收中介费
> > 	private void fare() {
> > 		System.out.println("收取中介费");
> > 		
> > 	}
> > 
> > }
> > 
> > ```
> >
> > ```java
> > package cn.leiyc.DynamicProxy;
> > 
> > import org.junit.jupiter.api.Test;
> > 
> > public class Client {
> > 	private Service service;
> > 	private ProxyInvocationHandler handler;
> > 	@Test
> > 	public void getRoom() {
> > 		service = new Service();
> > 		handler = new ProxyInvocationHandler();
> > 		handler.setRent(service);
> > 		Rent proxyRent = (Rent) handler.getProxy();
> > 		proxyRent.rent();
> > 		
> > 		
> > 	}
> > 
> > }
> > 
> > ```
> >
> > 动态代理一般代理一类事务，可以代理多个类



**有待深入**

