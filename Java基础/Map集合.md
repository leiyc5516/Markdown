![](E:\MarkdownPicture\440.jpg)





#### Collection接口以及对应的子接口都是存储的单个对象；为了满足存储二元偶对象（key,value）使用Map

* **Map的核心是通过key获取Value**	
* **开发中Collection保存数据是为了输出，Map保存数据是为了通过key查找**

**Map接口介绍**

* **Map是二元偶对象保存的最大父接口**

  ```java
  public interface Map<K,​V>
  ```

  该接口为一个独立父接口，并且在进行接口对象实例化的时候需要设置K与V也就是说整体操作的时候需要保存两个内容，在Map接口里面定义许多的操作方法,需要记住以下核心方法

* ```Java
  public void clear()//清空Map
  ```

* ```java
  public V put​(K key,V value)//向集合之中保存数据
  ```

* ```java
  public V get​(Object key)//根据key查询value
  ```

* ```java
  public Set<Map.Entry<K,​V>> entrySet()//将Map集合转换为set集合
  ```

* ```Java
  public boolean containsKey​(Object key)//查询key是否存在
  ```

* ```java
  public Set<K> keySet()//将Map集合的key转为set集合
  ```

* ```java
  public V remove​(Object key)//根据key删除指定的value
  ```

* Map中扩充静态方法of：

  ```java
  package map;
  import java.util.Map;
  
  public class ImplMap1 {
  
  	public static void main(String[] args) {
  		Map<String, Integer> user = Map.of("one",1,"tow",2);
  		System.out.println(user);
  	}
  
  }
  
  ```

* 按照key=value 存储里面的数据是使用of方法不允许key值重复的。
* **常用的Map的子类**
  * HashMap
  * HashTable
  * TreeMap
  * LinkedHashMap

### HashMap

* **HashMap是Map接口之中最为常见的一个子类**

**HashMap**

```java
public class HashMap<K,​V>
extends AbstractMap<K,​V>
implements Map<K,​V>, Cloneable, Serializable
```

| Modifier and Type | Method                                                  | Description                                                  |
| :---------------- | :------------------------------------------------------ | :----------------------------------------------------------- |
| `void`            | `clear()`                                               | Removes all of the mappings from this map.                   |
| `Object`          | `clone()`                                               | Returns a shallow copy of this `HashMap` instance: the keys and values themselves are not cloned. |
| `V`               | `compute(K key, BiFunction remappingFunction)`          | Attempts to compute a mapping for the specified key and its current mapped value (or `null` if there is no current mapping). |
| `V`               | `computeIfAbsent(K key, Function mappingFunction)`      | If the specified key is not already associated with a value (or is mapped to `null`), attempts to compute its value using the given mapping function and enters it into this map unless `null`. |
| `V`               | `computeIfPresent(K key, BiFunction remappingFunction)` | If the value for the specified key is present and non-null, attempts to compute a new mapping given the key and its current mapped value. |
| `boolean`         | `containsKey(Object key)`                               | Returns `true` if this map contains a mapping for the specified key. |
| `boolean`         | `containsValue(Object value)`                           | Returns `true` if this map maps one or more keys to the specified value. |
| `Set>`            | `entrySet()`                                            | Returns a [`Set`](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Set.html) view of the mappings contained in this map. |
| `V`               | `get(Object key)`                                       | Returns the value to which the specified key is mapped, or `null` if this map contains no mapping for the key. |
| `boolean`         | `isEmpty()`                                             | Returns `true` if this map contains no key-value mappings.   |
| `Set`             | `keySet()`                                              | Returns a [`Set`](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Set.html) view of the keys contained in this map. |
| `V`               | `merge(K key, V value, BiFunction remappingFunction)`   | If the specified key is not already associated with a value or is associated with null, associates it with the given non-null value. |
| `V`               | `put(K key, V value)`                                   | Associates the specified value with the specified key in this map. |
| `void`            | `putAll(Map m)`                                         | Copies all of the mappings from the specified map to this map. |
| `V`               | `remove(Object key)`                                    | Removes the mapping for the specified key from this map if present. |
| `int`             | `size()`                                                | Returns the number of key-value mappings in this map.        |
| `Collection`      | `values()`                                              | Returns a [`Collection`](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Collection.html) view of the values contained in this map. |

```java
package map;

import java.util.HashMap;
import java.util.Map;

public class ImplHashMap {

	public static void main(String[] args) {
		Map<Integer, String> user = new HashMap<Integer, String>();
		user.put(1, "one");
		String string = user.put(1, "1");
		user.put(2, "tow");
		user.put(3, "three");
		user.put(4, "four");
		user.put(5, null);
		user.put(null, "kong");
		System.out.println(user);
		System.out.println(string);
	}

}

```

```
{null=kong, 1=1, 2=tow, 3=three, 4=four, 5=null}
one

```



**key值和value在HashMap可以使null**

**当设置相同的key的时候value也是不会被抛弃的而是返回·**

```java
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }//无参构造loadFactor默认属性的内容0.75
```

```java
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }//key  hash赋值保证key值得单一性,使用putVal()方法处理利用Node节点存储
```

```java
 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

```

### 面试题：在进行HashMap   的put存储的时候，如何实现容量扩充？

* 在HashMap实现中有    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16作为初始化的容量配置，而这个常量**默认值为16个元素**
* 当保存的容量大于阀值（   static final float DEFAULT_LOAD_FACTOR = 0.75f;  容量\*阀值率=现在的容量\*0.75）这个时候扩容： 使用resize()方法扩充容量
* HashMap每次都是成倍扩充

### 面试题：请解释HashMap工作原理

* 在HashMap之中进行数据存储依然是利用node类实现，可以使用的数据结构有两种：链表和二叉树
* JDK1.8以后HashMap实现出现了改变，因为其要适应大数据海量数据问题，在HashMap中提供一个重要常量   static final int TREEIFY_THRESHOLD = 8;**没有超过阈值8使用链表存储，超过8转为红黑树存储**

* HashMap无序存储





#### LinkedHashMap HashMap虽然是Map集合常用子类，但是其本身所保存的数据都是无序的，所以有时候并不能满足一些有序存储，如果需要有序存储序偶对需要使用LinkedHashMap（基于链表实现）

#### LinkedHashMap 类实现

```java
public class LinkedHashMap<K,​V>
extends HashMap<K,​V>
implements Map<K,​V>
```

使用LinkedHashMap时数据量不宜太多。





#### **HashTable类 	不允许key与value为空**

### HashMap和HashTable的区别

* HashMap是线程不安全，异步执行；允许保存空
* HashTable是线程安全，同步执行；不允许保存空值





### Map.Entry内部结构：Map的子类所有的key和value都被封装到Map.Entry接口中

```java
public static interface Map.Entry<K,​V>
```

|       |                     |                                                              |
| :---- | ------------------- | ------------------------------------------------------------ |
| `K`   | `getKey()`          | Returns the key corresponding to this entry.                 |
| `V`   | `getValue()`        | Returns the value corresponding to this entry.               |
| `int` | `hashCode()`        | Returns the hash code value for this map entry.              |
| `V`   | `setValue(V value)` | Replaces the value corresponding to this entry with the specified value (optional operation). |





### 对于集合的输出而言，最标准的方法 就是使用Iterator接口完成，但是需要明确在Map中没有任何一个方法可以返回一个Iterator对象

#### **Map实际存储的是一株Map.Entry对象**Map依然是实现的单值保存

#### public Set<Map.Entry<K,V>> entrySet()//将全部的Map集合转换为set集合

![](E:\MarkdownPicture\MapCollection.png)

![](E:\MarkdownPicture\Maptoset.png)

### Map使用Iterator实现集合的输出，则必须按照如些操作

* #### 使用Map中的提供的entrySet（）将Map转换为Set集合

* #### 利用Set的Iterator（）方法转换将Set集合为Iterator接口实例

* #### 利用Iterator进行迭代输出获取每一组的Map.Entry对象，在通过getKey(),getValue获取数据

```java
package map;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class ImplCout {

	public static void main(String[] args) {
		Map<String, Integer> user = new HashMap<String, Integer>(5);
		user.put("one",1);
		user.put("tow",2);
		user.put("three",3);
		user.put("four",4);
		Set<Map.Entry<String, Integer>> set= user.entrySet();
		Iterator<Map.Entry<String, Integer>>iter= set.iterator();
		System.out.println("KEY"+"   "+"VALUE");
		while (iter.hasNext()) {
			Map.Entry<String, Integer> userEntry = iter.next();
			System.out.println(userEntry.getKey()+"   "+userEntry.getValue());
		}
	}

}

```

### 关于自定义key

```Java
package map;

import java.util.HashMap;
import java.util.Map;

class Person{
	String name;
	int age;
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Person other = (Person) obj;
		if (age != other.age)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}
}

public class ImplKey {
	public static void main(String[] args) {
		Map<Person, String> user = new HashMap<Person, String>();
		Person p = new Person("zhangsan", 12);
		user.put(p, "nan");
		System.out.println(user.get(new Person("zhangsan", 12)));
	}

}

```

### 自定义key   一定要覆写hashcode和equals方法

### 面试题：如果在HashMap进行数据处理操作时出现hash冲突，HashMap是如何解决的

* #### 为了保证程序的正常执行一旦发生冲突就把hash冲突转为链表保存







**Hash算法解决冲突的方法一般有以下几种常用的解决方法** 
**1， 开放定址法：** 
**所谓的开放定址法就是一旦发生了冲突，就去寻找下一个空的散列地址，只要散列表足够大，空的散列地址总能找到，并将记录存入** 
**公式为：fi(key) = (f(key)+di) MOD m (di=1,2,3,……,m-1)** 
**※ 用开放定址法解决冲突的做法是：当冲突发生时，使用某种探测技术在散列表中形成一个探测序列。沿此序列逐个单元地查找，直到找到给定的关键字，或者** 
**碰到一个开放的地址（即该地址单元为空）为止（若要插入，在探查到开放的地址，则可将待插入的新结点存人该地址单元）。查找时探测到开放的地址则表明表** 
**中无待查的关键字，即查找失败。** 
**比如说，我们的关键字集合为{12,67,56,16,25,37,22,29,15,47,48,34},表长为12。 我们用散列函数f(key) = key mod l2** 
**当计算前S个数{12,67,56,16,25}时，都是没有冲突的散列地址，直接存入：** 

**![这里写图片描述](https://img-blog.csdn.net/20170210213355178?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVpbmlr/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)** 
**计算key = 37时，发现f(37) = 1，此时就与25所在的位置冲突。** 
**于是我们应用上面的公式f(37) = (f(37)+1) mod 12 = 2。于是将37存入下标为2的位置：** 
**![这里写图片描述](https://img-blog.csdn.net/20170210213443522?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVpbmlr/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)**

**2， 再哈希法：** 
**再哈希法又叫双哈希法，有多个不同的Hash函数，当发生冲突时，使用第二个，第三个，….，等哈希函数**
**计算地址，直到无冲突。虽然不易发生聚集，但是增加了计算时间。**

**3， 链地址法：** 
**链地址法的基本思想是：每个哈希表节点都有一个next指针，多个哈希表节点可以用next指针构成一个单向链表，被分配到同一个索引上的多个节点可以用这个单向** 
**链表连接起来，如：** 
**键值对k2, v2与键值对k1, v1通过计算后的索引值都为2，这时及产生冲突，但是可以通道next指针将k2, k1所在的节点连接起来，这样就解决了哈希的冲突问题** 
**![这里写图片描述](https://img-blog.csdn.net/20170210213528336?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVpbmlr/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)**
**4， 建立公共溢出区：** 
**这种方法的基本思想是：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表**