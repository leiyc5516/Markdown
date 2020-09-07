![](E:\MarkdownPicture\440.jpg)

#### List是Collection的子接口：

* 可以允许有相同重复的元素

* ```java 
  public interface List<E> extends Collection<E>
  ```

* List 接口对Collection方法进行了扩充

  * ```Java
    public E get (int index)//获取索引的数据
    ```

  * ```java
    public E set(int index,E element)//修改索引的数据
    ```

  * ```Java
    public ListIterator<E>listIterator()//将集合变成ListIterator接口返回
    ```

* ArrayList是List 使用最多的子接口，但是在使用时候也是有前提要求

  * ArrayList在Java中的定义：

    * ```java
      public class ArrayList<E>
      extends AbstractList<E>//是AbstractList子类
      implements List<E>, RandomAccess, Cloneable, Serializable//可以随机访问也可以序列化
       //可以深克隆
      ```

    * ```java
      public abstract class AbstractList<E>
      extends AbstractCollection<E>//是AbstractCollection子类
      implements List<E>//实现list接口
      ```

    * 使用ArrayList实例化我们的List父类

      ```java
      package list;
      
      import java.util.ArrayList;
      import java.util.List;
      
      public class ImplList {
      	public static void main(String[] args) {
      		List<Integer> all = new ArrayList<Integer>();//为list父接口进行实例化
      		for (int i = 0; i < 10; i++) {
      			all.add(i);
      		}
      		for (int i = 0; i < 10; i++) {
      			all.add(i);
      		}
      		System.out.println(all);
      		
      	}
      }
      
      ```

      * 保存的顺序就是器存储的顺序：
      * List集合里面允许存在重复数据
      * 输出的时候可以利用Iterable定义的ForEach方法（非标准输出）

* ArrayList类似编写的链表但是其实ArrayList实际封装的数组

  * ArrayList无参构造的方法位空数组

  * ArrayList有参构造的方法设置一个大小
  * 在ArrayList中追加数据一旦数据大于数组大小，利用Array.copyof()增长数组长度，拷贝到新数组数组的开辟操作
  * 默认使用空数组，则会判断增长的容量和默认的容量大小，使用较大的数值进行新的数组开辟
  * 增长使用成倍的增长方式



#### ArrayList实现自定义的类的对象的保存

```java
package list;

import java.util.ArrayList;
import java.util.List;

public class ImplList {
	public static void main(String[] args) {
		List<person> all = new ArrayList<person>(10);//为list父接口进行实例化
		for (int i = 0; i < 10; i++) {
			person p = new person();
			p.setName("li"+i);
			p.setPassword("123"+i);
			all.add(p);
		}
		for (person person : all) {//读取每一个对象 
			System.out.println(person.getName()+"   "+person.getPassword());
		}
		
	}
}
class person{//自定义类
	String name;
	String password;
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
}
```

### 使用List保存自定义对象时使用contains或者remove方法时进行查询删除操作的时候一定要覆写equals（）方法





#### Linkedlist是基于链表实现

* LinkedList子类基本实现

```java
public class LinkedList<E>
extends AbstractSequentialList<E>
implements List<E>, Deque<E>, Cloneable, Serializable
```

![](E:\MarkdownPicture\as.png)

#### LinkedList的作用类似于ArrayList，但是内部构造方法是 不一样的，没有提供初始化大小；add方法的实现也是不一样的

```java
package list;

import java.util.LinkedList;
import java.util.List;

public class ImplLinkedList {
	public static void main(String[] args) {
		List<person> all = new LinkedList<person>();//为list父接口进行实例化
		for (int i = 0; i < 10; i++) {
			person p = new person();
			p.setName("li"+i);
			p.setPassword("123"+i);
			all.add(p);
		}
		all.forEach(System.out::println);
		
	}
}

```

#### ArrayList与LinkedList的区别

* ArrayList是基于数组实现的而LinkedList是基于链表实现的
* 在使用List中get()使用索引来实现读取数据的时候，ArrayList时间复杂度为O(1)而LinkedList时间复杂度为O(n)；n为链表的长度
* ArrayList默认的数组大小空间为10，空间不足的时候可以以二倍的方式增长。保存大数据时ArrayList可可能产生垃圾数据。这个时候可以使用LinkedList

#### Vector

* 继承结构与ArrayList一样

```java
package list;


import java.util.List;
import java.util.Vector;

public class ImplVector {
	public static void main(String[] args) {
		List<person> all = new Vector<person>(10);//为list父接口进行实例化
		for (int i = 0; i < 10; i++) {
			person p = new person();
			p.setName("li"+i);
			p.setPassword("123"+i);
			all.add(p);
		}

		for (person per : all) {
			System.out.println(per.getName()+"   "+per.getPassword());
		}
		
	}
}
```



#### Vector和ArrayList区别

* Vector是线程安全同步的
* ArrayList是线程不安全的性能高