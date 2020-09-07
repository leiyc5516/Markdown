![](E:\MarkdownPicture\440.jpg)

#### set集合不允许有重复的元素

* ```java
  public interface Set<E>
  extends Collection<E>
  ```

* set集合不可以使用get()方法，无法实现指定索引查找

* 提供了类似list集合的Of（）静态方法

* ```java
  import java.util.Set;
  
  public class ImplSet {
  	public static void main(String[] args) {
  		//保存重复元素到set
  		Set<String> all = Set.of("Hello","world","skr ","Hello","world");
  		all.forEach(System.out::println);
  	}
  }
  //Exception in thread "main" java.lang.IllegalArgumentException: duplicate element: Hello
  ```

* 常用的Set接口的子类：

  * HashSet：是Set中使用最多的一个子类，其最大特点是保存数据的**无**序性
  * hashset:实现去除重复元素利用object类中**hashcode(对象编码)和equals（对象比较）**

  ```java
  public class HashSet<E>
  extends AbstractSet<E>
  implements Set<E>, Cloneable, Serializable
  ```

  ```java
  package set;
  
  import java.util.HashSet;
  import java.util.Set;
  
  public class ImplHashSet {
  
  	public static void main(String[] args) {
  		Set<String> all = new HashSet<String>();
  		all.add("hash");
  		all.add("h");
  		all.add("ha");
  		all.add("has");
  		all.add("hash");
  		all.forEach(System.out::println);
  	}
  
  }
  
  ```

  

  * TreeSet是Set中使用最多的一个子类，其最大特点是保存数据的**有**序性；主要功能是排序操作   * 都将按照字母升序排序（**自定义类的实现需要实现comparable接口中的compareTo**）TreeSet是基于TreeMap实现

  ```java
  
  public class TreeSet<E>
  extends AbstractSet<E>
  implements NavigableSet<E>, Cloneable, Serializable
  ```

  ```java
  package set;
  
  import java.util.Set;
  import java.util.TreeSet;
  public class ImpTreeSet {
  
  	public static void main(String[] args) {
  		Set<String> all = new TreeSet<String>();
  		all.add("hash");
  		all.add("a");
  		all.add("d");
  		all.add("v");
  		all.add("c");
  		all.forEach(System.out::println);
  	}
  
  }
  
  ```

  

